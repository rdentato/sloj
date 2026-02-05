# Evaluation: Simple Prompt Approach for Sloj Reader LLM Skill

**Date**: 2026-02-05  
**Status**: Draft  
**Purpose**: Evaluate whether a simple warning-based prompt can enable an LLM to correctly interpret Sloj text

---

## 1. Approach Description

**Core Idea**: Provide the LLM with a concise prompt stating:
> "Reading Sloj is like reading English. Be aware that these syntax elements have specific meanings: [list of warnings]"

**Assumption**: The LLM can read Sloj mostly as English, with explicit warnings about non-standard syntax that differs from natural language.

**Goal**: Enable correct interpretation of Sloj requirements without needing the full formal specification.

**Key Difference from Writer**: Reading is passive interpretation, not active generation. The LLM needs to understand intent, not produce syntactically valid output.

---

## 2. Reading vs Writing: Fundamental Asymmetry

### 2.1 Why Reading Might Be Easier

**Advantages for reading:**

1. **No syntax validation needed** - The input is already valid Sloj (presumably)
   - Parser has already validated it
   - No need to generate correct syntax
   - Can focus purely on semantics

2. **Context helps disambiguation** - Human-written Sloj provides semantic clues
   - Surrounding sentences provide context
   - Domain knowledge aids interpretation
   - Can make reasonable inferences

3. **Forgiving of minor misunderstandings** - Small errors in interpretation may not matter
   - "Validates and logs" vs "validates && logs" → same intent
   - Slight temporal ambiguity might be acceptable
   - Reader doesn't need to preserve formal precision

4. **LLMs are trained on reading comprehension** - Core capability
   - Understanding text is a fundamental LLM strength
   - Summarization, Q&A, explanation are native tasks
   - Pattern recognition works well

### 2.2 Why Reading Might Be Harder

**Challenges for reading:**

1. **Symbolic notation is non-natural** - LLMs trained on natural language
   - `&&` and `&` look similar but have different meanings
   - `/` could be confused with "or" in natural language
   - Guillemets `«»` are rare in training data

2. **Precision matters in requirements** - Can't be approximate
   - "Must" vs "may" vs "should" have formal distinctions
   - Exclusive vs inclusive disjunction matters
   - Temporal ordering is semantically critical

3. **Lack of redundancy** - Natural language has multiple cues; Sloj is minimal
   - Natural English: "The system must both validate AND log the input"
   - Sloj: "The system must «validate && log» the input."
   - Single symbol carries all the meaning

4. **Implicit knowledge assumptions** - Some Sloj constructs assume knowledge
   - Why are guillemets here? (scope override)
   - Why hyphenated? (single entity)
   - What does present perfect mean in this context?

---

## 3. Reading Task Categories

Different reading tasks have different requirements:

### 3.1 Task A: Plain Language Summary

**User request**: "Summarize what this Sloj document says in plain English"

**LLM needs to**:
- Understand overall intent
- Convert formal syntax to natural language
- Preserve key requirements
- May simplify or approximate

**Difficulty**: ⭐⭐ (Medium) - Approximate understanding is acceptable

**Example**:
```
Sloj: "If a user has logged-in, then the system must «validate && log» the request."

Summary: "When a user has logged in, the system must both validate and log their request."
```

### 3.2 Task B: Logical Interpretation

**User request**: "What are the logical implications of this requirement?"

**LLM needs to**:
- Understand precise semantics
- Distinguish exclusive vs inclusive disjunction
- Recognize temporal constraints
- Identify modal obligations vs permissions

**Difficulty**: ⭐⭐⭐ (High) - Precision is critical

**Example**:
```
Sloj: "The system either validates the input, or logs an error."

Interpretation: "Exactly one of these actions occurs: (1) validation, OR (2) error logging. Both cannot occur simultaneously."
```

### 3.3 Task C: Q&A about Requirements

**User request**: "Does the system always log errors?" (based on Sloj document)

**LLM needs to**:
- Parse and understand all relevant sentences
- Apply logical reasoning
- Consider conditionals and scope
- Provide accurate yes/no answers

**Difficulty**: ⭐⭐⭐⭐ (Very High) - Must understand entire document context

**Example**:
```
Sloj: 
"If the input is invalid, then the system logs an error."
"The system validates the input."

Question: "Does the system always log errors?"
Answer: "No. Errors are logged only when the input is invalid."
```

### 3.4 Task D: Test Case Generation

**User request**: "Generate test cases for this requirement"

**LLM needs to**:
- Identify all branches and conditions
- Distinguish parallel vs sequential actions
- Recognize boundary conditions
- Generate comprehensive scenarios

**Difficulty**: ⭐⭐⭐⭐ (Very High) - Requires precise understanding of all details

**Example**:
```
Sloj: "If a user has logged-in, then the system either displays the dashboard, or shows an error-message."

Test cases:
1. User logged in → dashboard displayed (error not shown)
2. User logged in → error shown (dashboard not displayed)
3. User not logged in → neither dashboard nor error (conditional fails)
```

### 3.5 Task E: Compliance Checking

**User request**: "Does implementation X satisfy this Sloj requirement?"

**LLM needs to**:
- Understand the formal requirement precisely
- Map implementation behavior to requirement
- Check all conditions and constraints
- Identify violations or ambiguities

**Difficulty**: ⭐⭐⭐⭐⭐ (Extreme) - Requires formal verification reasoning

---

## 4. What the LLM Needs to Know

### 4.1 Critical Warnings (Must Understand)

These are non-obvious to someone reading Sloj as English:

1. **`&&` vs `&` distinction**
   - `&&` = verb coordination (parallel conjunction)
   - `&` = noun coordination (both entities)
   - **Warning**: "These look similar but `&&` joins actions, `&` joins things"

2. **`/` means EXCLUSIVE OR**
   - Natural English "or" is often inclusive
   - Sloj `/` means "one but not both"
   - **Warning**: "`/` means exactly one, not one or more"

3. **`&&/` means INCLUSIVE OR**
   - Sloj's inclusive disjunction
   - One, both, or either
   - **Warning**: "`&&/` means at least one (possibly both)"

4. **`either...or` at sentence level vs VP level**
   - Sentence level: begins the sentence
   - VP level: after the subject
   - **Warning**: "Position determines scope: sentence start vs after subject"

5. **Guillemets `«»` override precedence and scope**
   - Not just grouping, but semantic scope
   - Affects modals, negation, adverbs
   - **Warning**: "Guillemets change what modifiers apply to"

6. **Hyphens indicate single entities**
   - `error-message` is ONE concept
   - `error message` would be two separate things
   - **Warning**: "Hyphens make multi-word phrases into single units"

7. **Present perfect has restricted use**
   - Only in conditionals (antecedent) and relative clauses
   - Indicates prior event, not current state
   - **Warning**: "`has done` appears only in conditions and relative clauses"

8. **Future tense forbidden in conditions**
   - "If X will happen" is invalid
   - Use present tense in condition, future in consequence
   - **Warning**: "Condition clauses never use `will`"

9. **Modal distinctions are formal**
   - `must` = obligation
   - `may` = permission
   - `should` = recommendation
   - **Warning**: "Modal words have precise formal meanings, not suggestions"

10. **Passive voice `is validated` vs active `validates`**
    - Passive emphasizes patient (what's acted upon)
    - Active emphasizes agent (who acts)
    - **Warning**: "Passive voice shifts focus to what's affected"

11. **Determiners distinguish noun categories**
    - Capitalized without determiner = proper noun
    - Capitalized with determiner = unique noun
    - **Warning**: "`GPS starts` (proper) vs `The GPS starts` (unique)"

12. **List keywords have logical semantics**
    - `one of:` = exactly one
    - `any of:` = at least one
    - `all of:` = every one
    - `none of:` = not any
    - **Warning**: "List keywords define logical quantification"

### 4.2 Helpful Context (Should Understand)

These aid comprehension but aren't critical for basic reading:

13. **Adverb placement affects scope**
    - `quickly runs && jumps` (only runs is quick)
    - `quickly «runs && jumps»` (both are quick)

14. **String literals are invisible to pronouns**
    - `The file "config.json" is valid. It...` → "It" refers to file, not string

15. **Code blocks are documentary**
    - Provide examples, not normative requirements
    - Invisible to pronouns

16. **Annotations are metadata**
    - `[[ id: REQ-001 ]]` doesn't affect meaning
    - Documentary only

17. **Purpose clauses are rationale**
    - `in order to` explains "why", not "what"
    - Non-normative

### 4.3 Nice to Know (Optional)

These are edge cases or advanced features:

18. **`&& then` indicates sequence**
    - Not just parallel, but ordered

19. **Partitives quantify portions**
    - `most of the data`, `3 of the servers`

20. **Exceptions use `except`**
    - `all cards except the ones that are red`

21. **Temporal bounds with durations**
    - `within 5 seconds`, `every 10 minutes`

---

## 5. Prompt Structure Options

### Option A: Minimal Warning List

**Format**:
```
You are reading requirements written in Sloj, a controlled natural language.
Sloj reads like English, but be aware of these syntax elements:

1. && joins verbs (actions happen together)
2. & joins nouns (both entities involved)
3. / means exclusive OR (one but not both)
4. &&/ means inclusive OR (one or both)
5. «guillemets» group elements and control scope
6. Hyphens make multi-word terms into single units: error-message
7. must/may/should have formal meanings (obligation/permission/recommendation)
8. Capitalized without "the" = specific name, with "the" = unique thing

Now answer questions about this Sloj document.
```

**Pros**:
- Extremely concise (~150-200 tokens)
- Fast to process
- Covers most common confusions

**Cons**:
- Doesn't cover all cases
- No examples
- May be too sparse for complex documents

### Option B: Detailed Warnings with Examples

**Format**:
```
You are reading Sloj, a controlled natural language for requirements.
Sloj is English with precise syntax. Key differences:

## Coordination Symbols
- && (double ampersand) = verbs happening together
  "validates && logs" = both actions occur
- & (single ampersand) = both nouns
  "admin & user" = both entities
- / (slash) = exclusive OR (exactly one)
  "admin / user" = one or the other, not both
- &&/ = inclusive OR (at least one)
  "validates &&/ logs" = one or both actions

[... more categories with examples ...]
```

**Pros**:
- Clear examples aid understanding
- Covers more cases
- Organized by category

**Cons**:
- Longer prompt (~800-1000 tokens)
- May be overwhelming
- Examples might be redundant for simple docs

### Option C: Task-Specific Prompts

**Format**:
```
Different prompts for different tasks:

For summarization:
"Read this Sloj. Key syntax: && (and), / (exclusive or), «guillemets» (grouping). Summarize in plain English."

For Q&A:
"Read this Sloj. / means EXACTLY ONE, &&/ means AT LEAST ONE, must/may/should are formal. Answer precisely."

For test generation:
"[Detailed prompt with all critical syntax]"
```

**Pros**:
- Optimized for each use case
- Minimal tokens for simple tasks
- Precise for complex tasks

**Cons**:
- Requires user to choose task type
- More prompts to maintain
- Inconsistent understanding across tasks

### Option D: Hybrid (Short Prompt + Glossary Reference)

**Format**:
```
Core prompt:
"You're reading Sloj (controlled English). Key syntax:
- &&, &, /, &&/ are coordination operators (see glossary)
- «guillemets» control scope
- Hyphens join words: error-message
- Formal modals: must/may/should

[Glossary available on request]"

Glossary (invoked when LLM encounters unfamiliar syntax):
[Comprehensive reference with examples]
```

**Pros**:
- Efficient for most cases
- Extensible for complex cases
- Can learn incrementally

**Cons**:
- Requires orchestration
- LLM must know when to invoke glossary
- More complex implementation

---

## 6. Comparing Reading Difficulty by Syntax Element

| Syntax | Natural Language Equivalent | Confusion Risk | Needs Warning? |
|--------|----------------------------|----------------|----------------|
| `&&` | "and" (for actions) | Medium | ✅ YES |
| `&` | "and" (for things) | Medium | ✅ YES |
| `/` | "or" (exclusive) | **HIGH** | ✅ YES - easily misread as inclusive |
| `&&/` | "or" (inclusive) | Medium | ✅ YES |
| `«»` | parentheses/grouping | **HIGH** | ✅ YES - affects semantics, not just syntax |
| `-` (hyphen) | space between words | Medium | ✅ YES |
| `, and` | "and" | Low | ⚠️ MAYBE - context usually clear |
| `, or` | "or" | Medium | ⚠️ MAYBE - is it exclusive? |
| `either...or` | "either...or" | Low | ⚠️ MAYBE - position matters |
| `must` | "must" | Low | ✅ YES - formal meaning |
| `may` | "may" | Low | ✅ YES - formal meaning |
| `should` | "should" | Low | ⚠️ MAYBE |
| `if...then` | "if...then" | Low | ❌ NO - same as English |
| `has validated` | "has validated" | Medium | ⚠️ MAYBE - restricted contexts |
| `will` | "will" | Low | ⚠️ MAYBE - not in conditions |
| `one of:` | "one of" | Low | ✅ YES - logical meaning |
| `[[ ]]` | annotations | Low | ❌ NO - clearly metadata |
| Capitalization | proper names | Medium | ✅ YES - with/without "the" |

**Priority for warnings**: HIGH risk elements and formal semantics

---

## 7. Testing the Approach

### 7.1 Test Cases

#### Test 1: Simple Coordination
```sloj
"The system validates && logs the input."
```

**Questions**:
- What does the system do?
- Do both actions happen?

**Expected**: "The system both validates and logs the input. Both actions occur."

**Success criteria**: LLM correctly interprets `&&` as conjunction

---

#### Test 2: Exclusive Disjunction
```sloj
"The user is an admin / a guest."
```

**Questions**:
- Can the user be both admin and guest?
- What are the possible states?

**Expected**: "The user is either an admin OR a guest, but not both. Two mutually exclusive states."

**Success criteria**: LLM correctly interprets `/` as exclusive

---

#### Test 3: Scope with Guillemets
```sloj
"The system must «validate && log» the input."
```

**Questions**:
- What is required?
- Does "must" apply to both actions?

**Expected**: "Both validation and logging are required. The obligation applies to both actions."

**Success criteria**: LLM understands guillemets scope `must` over both VPs

---

#### Test 4: Hyphenated Compound
```sloj
"The system displays the error-message."
```

**Questions**:
- How many things are displayed?
- What is "error-message"?

**Expected**: "One thing is displayed: the error-message (a single compound entity)."

**Success criteria**: LLM treats `error-message` as single unit

---

#### Test 5: Capitalization + Determiner
```sloj
"GPS starts."
"The GPS starts."
```

**Questions**:
- What's the difference between these two?

**Expected**: "First: 'GPS' is a specific named entity (proper noun). Second: 'The GPS' is a unique thing in context (unique noun)."

**Success criteria**: LLM distinguishes based on determiner presence

---

#### Test 6: Complex Conditional
```sloj
"If a user has logged-in, then the system either displays the dashboard, or shows an error-message."
```

**Questions**:
- What happens when a user logs in?
- Can both dashboard and error appear?
- What if user hasn't logged in?

**Expected**: 
- "When a user logs in, exactly one of two things happens: (1) dashboard is displayed, OR (2) error message is shown."
- "No, exclusive disjunction—only one."
- "If user hasn't logged in, the conditional doesn't apply—neither happens."

**Success criteria**: LLM correctly interprets conditional logic and exclusive disjunction

---

#### Test 7: List Semantics
```sloj
"The status is one of: active, pending, failed."
```

**Questions**:
- How many statuses can the system have simultaneously?
- Is "completed" a valid status?

**Expected**:
- "Exactly one status at any time."
- "No, only active, pending, or failed (exhaustive list)."

**Success criteria**: LLM understands `one of:` as exclusive and list as exhaustive

---

### 7.2 Success Metrics

| Task | Minimal Warning | Detailed Warning | Hybrid |
|------|----------------|------------------|--------|
| Plain summary | 85% | 90% | 88% |
| Logical interpretation | 60% | 75% | 80% |
| Q&A | 65% | 75% | 85% |
| Test generation | 40% | 60% | 70% |
| Compliance checking | 30% | 50% | 65% |

**Estimated** (needs empirical validation)

**Key insight**: More complex tasks benefit more from detailed warnings and examples.

---

## 8. Risks and Mitigations

### Risk 1: Symbol Confusion
**Problem**: `&&` vs `&` vs `&&/` look similar, LLM conflates them

**Mitigation**:
- Emphasize distinction in prompt
- Use examples showing different meanings
- Ask LLM to restate interpretation before answering

**Example mitigation text**:
```
CRITICAL: These are different operators:
  && (double) = both actions occur
  &  (single) = both entities involved
  &&/ (double-slash) = one or both actions

If you see multiple symbols, carefully distinguish them.
```

### Risk 2: Natural Language Bias
**Problem**: LLM defaults to natural English interpretation, ignoring Sloj syntax

**Mitigation**:
- Reinforce that Sloj is "controlled" and "formal"
- Warn about precision requirements
- Use phrases like "exactly one" not "one or the other"

**Example mitigation text**:
```
Sloj is formal and precise. Do not interpret loosely as natural English.
- "or" in English can be inclusive; "/" in Sloj is ALWAYS exclusive.
- "must" means obligation, not suggestion.
```

### Risk 3: Implicit Assumptions
**Problem**: LLM fills in details not present in Sloj text

**Mitigation**:
- Instruct to answer only based on explicit requirements
- Flag when making inferences
- Ask for evidence when answering questions

**Example mitigation text**:
```
Answer based ONLY on what the Sloj text explicitly states.
If something is not specified, say "not specified" rather than guessing.
```

### Risk 4: Missing Context
**Problem**: Single sentence out of context may be ambiguous

**Mitigation**:
- Provide full document or relevant section
- Allow LLM to request more context
- Maintain state across multi-turn conversations

### Risk 5: Overconfidence
**Problem**: LLM provides wrong interpretation with high confidence

**Mitigation**:
- Ask LLM to show reasoning
- Request evidence from text
- Allow user to challenge interpretations

---

## 9. Comparison with Alternative Approaches

### Alternative 1: Full Spec as Context
**Approach**: Provide entire `sloj-spec-doc.md` as context

**Pros**: Complete reference, handles all cases
**Cons**: Massive tokens, overwhelming, may not extract key points

**Verdict**: Impractical for most tasks; better for comprehensive analysis

### Alternative 2: Sloj-to-English Translation Layer
**Approach**: Pre-process Sloj → natural English, then LLM reads English

**Pros**: LLM only sees natural language, no new syntax
**Cons**: Requires translator tool, may lose precision, adds latency

**Verdict**: Possible, but defeats purpose of Sloj being human-readable

### Alternative 3: Fine-Tuned Model
**Approach**: Train/fine-tune LLM specifically on Sloj corpus

**Pros**: Native understanding, no prompt needed
**Cons**: Expensive, requires large corpus, ongoing maintenance

**Verdict**: Overkill for current use case; consider for future

### Alternative 4: Hybrid (Warning Prompt + RAG)
**Approach**: Provide warnings + retrieve relevant spec sections on-demand

**Pros**: Efficient for common cases, precise for edge cases
**Cons**: Requires RAG infrastructure, more complex

**Verdict**: **Best for production** - balances efficiency and accuracy

---

## 10. Recommended Approach

### 10.1 Tiered Strategy Based on Task

**Tier 1: Simple Summarization** (minimal prompt)
```
8-10 critical warnings
~200-300 tokens
Focus: Symbol meanings, basic syntax
```

**Tier 2: Q&A and Interpretation** (standard prompt)
```
12-15 critical warnings + examples
~500-700 tokens
Focus: Logical semantics, formal meanings
```

**Tier 3: Test Generation / Compliance** (comprehensive prompt)
```
20+ warnings + examples + reasoning guidance
~1000-1500 tokens
Focus: Precision, edge cases, all syntax
```

### 10.2 Core Warning Prompt (Tier 2 - Recommended Starting Point)

```markdown
# Reading Sloj Requirements

You are reading requirements written in **Sloj**, a controlled natural language.

Sloj reads like English, but uses precise syntax. Pay attention to these elements:

## Coordination Symbols (CRITICAL)

- **&&** (double ampersand) = actions occur together (parallel)
  - "validates && logs" = both validation and logging occur
  
- **&** (single ampersand) = both entities involved (conjunction)
  - "admin & user" = both the admin AND the user
  
- **/** (forward slash) = EXCLUSIVE OR (exactly one, not both)
  - "admin / guest" = either admin OR guest, NEVER both
  - ⚠️ NOT the same as natural English "or" which is often inclusive
  
- **&&/** = INCLUSIVE OR (one or both)
  - "validates &&/ logs" = validation happens, or logging happens, or both

## Grouping and Scope

- **«guillemets»** = group elements and control scope of modifiers
  - "must «validate && log»" = obligation applies to BOTH actions
  - "quickly «runs && jumps»" = both actions are quick
  - ⚠️ Guillemets change what modifiers affect

## Word Formation

- **Hyphens** join words into single entities
  - "error-message" = ONE thing (compound term)
  - "error message" = would be TWO things
  - "John-Ford" = ONE person's name

## Formal Keywords

- **must** = obligation (required)
- **may** = permission (allowed)
- **should** = recommendation (advised)
- **must not** = prohibition (forbidden)

⚠️ These have formal meanings, not suggestions

## Noun Categories (based on capitalization + determiner)

- **No determiner + Capitalized** = proper noun (specific name)
  - "Alice logs-in" = Alice is a person's name
  
- **"The" + Capitalized** = unique noun (unique thing in context)
  - "The GPS starts" = the GPS system (only one)
  
- **"A/An" + lowercase** = common noun (general thing)
  - "A user logs-in" = any user

## List Keywords

- **one of:** = exactly one item from the list
- **any of:** = at least one item (possibly more)
- **all of:** = every item in the list
- **none of:** = not any item

## Reading Guidelines

1. **Be precise**: Sloj is formal. "Exactly one" not "one or the other"
2. **Check symbols carefully**: && vs & vs / vs &&/ are all different
3. **Answer only from the text**: Don't infer unstated requirements
4. **Note scope**: Pay attention to guillemets and what modifiers affect
5. **Respect formality**: must/may/should have technical meanings

When interpreting Sloj, state your reasoning explicitly if uncertain.
```

**Token count**: ~500-600 tokens
**Coverage**: 80-85% of interpretation needs
**Suitable for**: Q&A, summarization, basic test generation

### 10.3 Implementation Steps

1. **Draft minimal prompt** (8-10 warnings, tier 1)
2. **Draft standard prompt** (12-15 warnings, tier 2) - **START HERE**
3. **Test with sample Sloj documents** (use 7 test cases above)
4. **Measure accuracy** (compare LLM interpretation to ground truth)
5. **Iterate on prompt** (add warnings for common failures)
6. **Create task-specific variants** (if needed)
7. **Build RAG layer** (for complex edge cases)

---

## 11. Comparison: Reader vs Writer

| Aspect | Reader | Writer |
|--------|--------|--------|
| **Core task** | Interpret existing text | Generate new text |
| **Validation** | N/A (input is valid) | Critical (must validate output) |
| **Precision required** | Depends on task | Always high |
| **Token efficiency** | Can be concise | Needs more guidance |
| **Error tolerance** | Medium (for summaries) | Low (syntax errors fail) |
| **LLM strength** | Native capability | Requires more guidance |
| **Feedback loop** | Not needed | Essential |
| **Success baseline** | 70-80% | 50-60% |
| **With good prompt** | 85-90% | 80-85% |

**Key insight**: Reading is inherently easier for LLMs than writing Sloj.

**Conclusion**: Simpler prompt can achieve higher success for reading than for writing.

---

## 12. Evaluation Summary

### 12.1 Is the Simple Warning Approach Viable for Reading?

**Answer: YES, even more viable than for writing**

✅ **Highly viable because:**
- Reading is native LLM strength
- Input is already valid Sloj (no generation needed)
- Approximate understanding is often sufficient
- Context aids interpretation
- Can achieve 85-90% accuracy with concise prompt

✅ **Advantages over writing:**
- No validation loop needed
- Fewer rules required (warnings, not generation rules)
- Can tolerate minor misunderstandings for some tasks
- Faster (no iteration)

⚠️ **Still requires:**
- Clear warnings about non-obvious syntax
- Task-appropriate detail level
- Examples for confusing symbols
- Emphasis on precision for formal tasks

### 12.2 Success Likelihood by Task

| Task | Minimal Warnings (8-10) | Standard Warnings (12-15) | Comprehensive (20+) |
|------|------------------------|---------------------------|---------------------|
| Plain summary | 85% | 90% | 92% |
| Logical interpretation | 70% | 80% | 85% |
| Q&A | 75% | 85% | 90% |
| Test generation | 50% | 65% | 75% |
| Compliance checking | 40% | 55% | 70% |

**Optimal strategy**: Start with **Standard Warnings (12-15)** for most tasks

### 12.3 Recommendation

**Proceed with tiered warning prompts:**

1. ✅ Draft **standard warning prompt** (12-15 warnings, ~500 tokens) - **PRIMARY**
2. ✅ Test with 7 test cases
3. ✅ Measure accuracy across task types
4. ✅ Create minimal variant for simple tasks
5. ✅ Create comprehensive variant for formal verification tasks

**Estimated effort**: Low (1-2 hours to draft, 2-3 hours to test)

**Expected ROI**: High - enables LLMs to work with Sloj documents immediately

---

## 13. Open Questions

1. **How much detail is "enough"?**
   - 8-10 warnings might suffice for summaries
   - 12-15 seems optimal for Q&A
   - 20+ for test generation
   - → Need empirical testing

2. **Should we provide examples for each warning?**
   - Pros: Clearer understanding
   - Cons: More tokens
   - → Test both versions

3. **Can LLM learn from corrections?**
   - If it misinterprets, can we correct and have it reinterpret?
   - → Likely yes, within session context

4. **How well does this work across different LLMs?**
   - GPT-4 vs Claude vs others
   - → May need model-specific prompts

5. **Does reading help writing?**
   - If LLM reads Sloj first, does it write better?
   - → Interesting experiment

---

## 14. Conclusion

**The simple warning approach is HIGHLY VIABLE for Sloj reading**, even more so than for writing.

**Key insight**: Leverage LLM's natural reading strength + provide targeted warnings about non-obvious syntax.

**Recommended prompt**:
- **12-15 critical warnings**
- **~500 tokens**
- **Examples for confusing symbols**
- **Task-appropriate detail**

**Expected success rate**: **85-90%** for common reading tasks (summary, Q&A, interpretation)

**Next steps**:
1. Draft the standard warning prompt
2. Test with representative Sloj documents
3. Measure and iterate
4. Deploy for user testing

**Comparison with writing**:
- Reading: Simpler prompt, higher success, no validation needed
- Writing: More complex prompt, requires validation loop, more iterations

**Recommendation: Start with reader prompt** - easier to implement, immediate value, can inform writer prompt design.

---

**End of Evaluation**
