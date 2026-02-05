# Evaluation: Simple Prompt Approach for Sloj Writer LLM Skill

**Date**: 2026-02-05  
**Status**: Draft  
**Purpose**: Evaluate whether a simple rule-based prompt can enable an LLM to write valid Sloj text

---

## 1. Approach Description

**Core Idea**: Provide the LLM with a concise prompt stating:
> "Writing in Sloj is like writing in English, but with these special rules: [list of rules]"

**Assumption**: The LLM's existing knowledge of English grammar and requirements writing can be constrained by explicit rules to produce valid Sloj.

**Goal**: Generate valid Sloj text without needing to understand the full formal specification.

---

## 2. Feasibility Analysis

### 2.1 What Makes This Approach Viable

**Strengths:**

1. **Sloj is English-based**: Most Sloj constructs align with natural English
   - Sentence structure is familiar: "A user logs-in."
   - Conditionals use natural keywords: "if...then", "whenever"
   - Modals are standard English: "must", "may", "should"

2. **Differences are explicit and enumerable**: The deviations from English are well-defined
   - Coordination uses symbols: `&&`, `&`, `/`, `&&/`
   - Hyphenation rules are mechanical
   - Guillemets have clear usage patterns

3. **LLMs are good at rule-following**: Modern LLMs can follow explicit formatting rules
   - Proven success with JSON, XML, code generation
   - Can maintain consistency across a document
   - Can apply mechanical transformations

4. **Incremental correction is possible**: If the LLM makes mistakes, rules can be refined
   - Add examples of common errors
   - Emphasize frequently violated rules
   - Use few-shot examples

### 2.2 What Makes This Approach Challenging

**Challenges:**

1. **Rule interactions are complex**: Some rules depend on context
   - Modal scope requires understanding VP coordination
   - Pronoun resolution requires tracking antecedents
   - Determiner choice depends on noun category (common/unique/mass)

2. **Implicit knowledge requirements**: Some aspects require semantic understanding
   - When to use passive vs active voice
   - How to structure complex conditionals
   - What constitutes a "single entity" for hyphenation

3. **Edge cases are numerous**: The spec has many special cases
   - Present perfect only in conditionals and relative clauses
   - Future tense forbidden in condition clauses
   - Non-of PPs in object position require guillemets

4. **Lexicon dependency**: Valid Sloj requires words from the lexicon
   - The LLM can't know which nouns are common vs mass vs unique
   - Verb argument compatibility is lexicon-dependent
   - This requires external validation (can't be prompt-only)

---

## 3. Rule Categories for the Prompt

To evaluate completeness, let's categorize the rules the LLM needs:

### 3.1 Critical Rules (Must-Have)

These rules, if violated, produce invalid Sloj that won't parse:

1. **Coordination operators**
   - VP: `&&`, `&& then`, `&&/`, `either...or`, `neither...nor`
   - NP: `&`, `/`, `&/`
   - Sentence: `, and`, `, or`, `, but`, `, whereas`

2. **Guillemets usage**
   - Override precedence: `A &&/ «B && C»`
   - Scope modifiers: `quickly «runs && jumps»`
   - Attach PPs to object NPs: `sends «a message to the server»`

3. **Hyphenation rules**
   - Multi-word entities: `error-message`, `session-timeout`
   - Multi-word proper nouns: `John-Ford`, `New-York`
   - Reflexive verbs: `self-authenticate`

4. **Determiner presence for noun categories**
   - Proper nouns: NO determiner (`Alice runs`)
   - Unique nouns: WITH determiner (`The GPS starts`)
   - Common nouns: WITH determiner (`A user logs-in`)

5. **Modal and negation syntax**
   - Modals: `must`, `must not`, `may`, `should`, `is able to`
   - VP negation: `does not`, `do not` (NOT with modals)

6. **Temporal restrictions**
   - Present perfect only in: conditionals (antecedent) and relative clauses
   - Future forbidden in: conditional antecedents, temporal prefixes
   - Use `has`/`have` + past participle for present perfect

7. **List syntax**
   - Inline: `one of: A, B, C.`
   - Markdown: colon at EOL, then `- item.` lines
   - Keywords: `one of`, `any of`, `all of`, `none of`

### 3.2 Important Rules (Should-Have)

These improve quality and reduce semantic errors:

8. **Sentence coordination precedence**
   - Disjunctive Normal Form: `(A and B) or (C and D)`
   - Disjunction outer, conjunction inner

9. **VP/NP list coordination**
   - Parallel (default): unmarked items
   - Sequential: `then` markers
   - Exclusive: `either:` + `or` markers
   - Inclusive: `or` markers without `either:`

10. **Prepositional phrase attachment**
    - Of-clauses bind to noun
    - Non-of PPs bind to verb (default)
    - Object position requires guillemets for non-of PPs

11. **Passive voice structure**
    - `is/are` + past participle
    - Optional agent: `by NP`
    - With modals: `must be validated`

12. **String literals and code blocks**
    - Invisible to pronoun resolution
    - Use for technical notation: `"active"`, `` `f(x)` ``

### 3.3 Refinement Rules (Nice-to-Have)

These improve style and readability:

13. **Pronoun usage**
    - Resolve to nearest matching antecedent
    - Gender and number agreement
    - Use explicit nouns when ambiguous

14. **Adverb placement**
    - Pre-verbal or post-verbal
    - At most one per VP
    - Scope with guillemets for groups

15. **Purpose clauses**
    - Non-normative: `in order to VP`
    - Can appear prefix, suffix, or incidental

16. **Annotations**
    - Metadata: `[[ key: value ]]`
    - Precede the annotated element

---

## 4. Prompt Structure Options

### Option A: Flat Rule List

**Format:**
```
You are writing requirements in Sloj, a controlled natural language.
Sloj is like English, but with these rules:

1. Use && for parallel conjunction of verbs: "validates && logs"
2. Use & for conjunction of nouns: "admin & auditor"
3. Use hyphens for multi-word entities: "error-message"
[... 40+ more rules ...]
```

**Pros:**
- Simple and direct
- Easy to maintain
- LLM can scan all rules

**Cons:**
- Long prompt (tokens)
- No structure/grouping
- Hard to prioritize
- Rules might conflict or confuse

### Option B: Categorized Rules with Examples

**Format:**
```
You are writing requirements in Sloj, a controlled natural language.
Sloj is English with controlled syntax. Follow these categories:

## CRITICAL: Coordination (use symbols, not words)
- Verbs: && (parallel), && then (sequential), &&/ (inclusive or)
  Example: "The system validates && logs the input."
- Nouns: & (and), / (exclusive or), &/ (inclusive or)
  Example: "The admin & auditor review the log."
[... grouped rules ...]
```

**Pros:**
- Organized by importance
- Examples aid understanding
- Can include rationale
- Easier to scan

**Cons:**
- Longer prompt
- Still many rules
- May be overwhelming

### Option C: Two-Tier Prompt (Core + Reference)

**Format:**
```
Core prompt (always included):
- 10-15 most critical rules
- Brief, actionable
- Covers 80% of common cases

Extended reference (included on-demand):
- Full rule set
- Edge cases
- Complex examples
- Invoked when LLM uncertain or user requests
```

**Pros:**
- Efficient token usage
- Focuses on common cases
- Extensible for complex documents

**Cons:**
- Requires orchestration
- LLM might not know when to invoke reference
- More complex implementation

### Option D: Few-Shot Learning

**Format:**
```
Here are examples of requirements in English and their Sloj translations:

English: "The user logs in and the system verifies credentials or times out."
Sloj: "A user logs-in, and the system verifies credentials, or the connection times-out."

English: "The system must validate and log the error message."
Sloj: "The system must «validate && log» the error-message."

[5-10 more examples covering key patterns]

Now convert these requirements to Sloj...
```

**Pros:**
- Leverages LLM's pattern recognition
- Natural learning from examples
- Less explicit rule memorization
- Can show context

**Cons:**
- Examples must be carefully chosen
- Doesn't cover all cases
- May not generalize to novel constructs
- Requires more prompt tokens

---

## 5. Validation Strategy

**Key Insight**: The LLM-generated Sloj MUST be validated externally.

### 5.1 Validation Layers

1. **Syntax validation** (parser)
   - Checks grammar conformance
   - Detects parse errors
   - Fast, deterministic

2. **Semantic validation** (linter)
   - Checks agreement, references, scope
   - Detects logical errors
   - Lexicon-dependent

3. **Round-trip validation** (optional)
   - Parse → logic → back to Sloj
   - Ensures semantic preservation
   - Slower, requires logic layer

### 5.2 Feedback Loop

```
User request → LLM (with prompt) → Sloj draft
                                      ↓
                              Syntax validator
                                      ↓
                              [Valid] → Output
                                      ↓
                            [Invalid] → Error report
                                      ↓
                              LLM (retry with errors)
                                      ↓
                              Sloj revised draft
```

**Critical**: The prompt should instruct the LLM to:
- Expect validation feedback
- Iterate on errors
- Ask clarifying questions when uncertain

---

## 6. Testing the Approach

### 6.1 Test Cases

To evaluate effectiveness, test with:

1. **Simple sentences**
   - "The user logs in."
   - Expected: "A user logs-in."

2. **Coordination**
   - "The system validates and logs the input."
   - Expected: "The system validates && logs the input."

3. **Conditionals**
   - "If the card is valid, the system accepts it."
   - Expected: "If the card is valid, then the system accepts the card."

4. **Complex requirements**
   - "When a user logs in, the system either displays the dashboard or shows an error message, depending on their authentication status."
   - Expected: Proper DNF with hyphens, determiners, coordination

5. **Edge cases**
   - Present perfect in conditions
   - Multi-word proper nouns
   - Modal scope over coordinated VPs

### 6.2 Success Metrics

- **Syntax correctness**: % of generated sentences that parse
- **Semantic correctness**: % that pass linter
- **Human readability**: Subjective quality assessment
- **Iteration count**: How many retries needed to reach valid Sloj
- **Coverage**: % of Sloj constructs the LLM can produce

---

## 7. Risks and Mitigations

### Risk 1: Rule Overload
**Problem**: Too many rules → LLM ignores some or gets confused

**Mitigation**:
- Prioritize critical rules
- Use tiered prompts
- Add examples for complex rules

### Risk 2: Semantic Gaps
**Problem**: LLM generates syntactically valid but semantically incorrect Sloj

**Mitigation**:
- Require external validation
- Provide feedback loop
- Include semantic guidance in prompt

### Risk 3: Lexicon Ignorance
**Problem**: LLM doesn't know which words are in the Sloj lexicon

**Mitigation**:
- Provide common lexicon words in prompt
- Allow user to specify domain lexicon
- Validate and iterate

### Risk 4: Novel Constructs
**Problem**: User requirement doesn't match any example or rule pattern

**Mitigation**:
- Instruct LLM to ask clarifying questions
- Allow fallback to simpler constructs
- Provide escape hatch (mark as "needs review")

### Risk 5: Inconsistent Style
**Problem**: LLM uses different patterns for similar requirements across a document

**Mitigation**:
- Provide document-level context
- Maintain glossary of terms used
- Enforce terminology consistency

---

## 8. Comparison with Alternative Approaches

### Alternative 1: Full Spec in Context
**Approach**: Provide entire `sloj-spec-doc.md` as context

**Pros**: Complete information, handles all cases
**Cons**: Massive token usage, overwhelming, LLM may not extract relevant rules

**Verdict**: Impractical for real-time use; better for one-off complex documents

### Alternative 2: Interactive Dialog
**Approach**: LLM asks questions, user provides English, LLM converts incrementally

**Pros**: Adapts to user needs, learns from corrections
**Cons**: Slower, requires user expertise, more interaction overhead

**Verdict**: Good for learning/teaching, less efficient for production

### Alternative 3: Grammar-Based Generation
**Approach**: LLM generates from grammar rules directly (BNF-based)

**Pros**: Guaranteed syntactic correctness
**Cons**: Requires formal grammar parsing capability, less natural, limited creativity

**Verdict**: Suitable for mechanical translation, not for creative requirements writing

### Alternative 4: Hybrid (Rule Prompt + Examples + Validation)
**Approach**: Combine simple rules, few-shot examples, and mandatory validation loop

**Pros**: Balances guidance and flexibility, learns from errors
**Cons**: More complex orchestration, requires tooling

**Verdict**: **Most promising** - leverages LLM strengths while ensuring correctness

---

## 9. Recommended Approach

### 9.1 Hybrid Strategy

**Core Components:**

1. **Concise rule prompt** (15-20 critical rules)
   - Categorized by importance
   - Each rule with 1 example
   - ~1000-1500 tokens

2. **Few-shot examples** (5-7 conversions)
   - Cover common patterns
   - Show before/after
   - ~500-800 tokens

3. **Validation feedback loop**
   - Parse → report errors
   - LLM retries with error context
   - Max 2-3 iterations

4. **Domain context** (optional)
   - Project-specific lexicon (100-200 common words)
   - Terminology glossary
   - Existing Sloj snippets from project

**Total prompt size**: ~2000-3000 tokens (fits in most LLM context windows)

### 9.2 Prompt Template Structure

```markdown
# Role
You are a requirements engineer writing specifications in Sloj, a controlled natural language.

# Task
Convert the user's requirements from English to valid Sloj syntax.

# Core Rules (CRITICAL - always apply)
[15-20 rules with examples]

# Examples
[5-7 before/after conversions]

# Process
1. Read the user's requirement
2. Identify key entities, actions, and constraints
3. Apply Sloj rules to generate the specification
4. If uncertain, ask clarifying questions
5. Expect validation feedback and iterate if needed

# Domain Context (for this project)
[Project-specific lexicon and terminology]
```

### 9.3 Implementation Steps

1. **Draft the core rule prompt** (20 most critical rules)
2. **Select few-shot examples** (covering 80% of patterns)
3. **Build validation pipeline** (parser + linter integration)
4. **Test with sample requirements** (iterate on prompt based on results)
5. **Add domain context mechanism** (project-specific lexicon injection)
6. **Deploy with feedback loop** (LLM ↔ validator iterations)

---

## 10. Evaluation Summary

### 10.1 Is the Simple Prompt Approach Viable?

**Answer: YES, with caveats**

✅ **Viable because:**
- Sloj is close enough to English that LLMs can adapt
- Critical rules are explicit and enumerable
- Modern LLMs are good at rule-following
- Validation can catch errors

⚠️ **But requires:**
- **Mandatory validation loop** - LLM output must be checked
- **Prioritized rule set** - Focus on critical rules, not all edge cases
- **Few-shot examples** - Aid pattern recognition
- **Domain context** - Project-specific lexicon
- **Iteration tolerance** - Accept 1-3 retry cycles

❌ **Not sufficient alone:**
- Rules-only prompt will miss edge cases
- Lexicon dependency requires external validation
- Complex semantic requirements need human review

### 10.2 Success Likelihood

**Estimated success rates** (based on prompt quality and LLM capability):

| Category | Simple Rules Only | Rules + Examples | Hybrid (Rules + Examples + Validation) |
|----------|------------------|------------------|----------------------------------------|
| Simple sentences | 70% | 85% | 95% |
| Coordination | 50% | 70% | 90% |
| Conditionals | 60% | 75% | 90% |
| Complex requirements | 30% | 50% | 80% |
| Edge cases | 10% | 30% | 70% |
| **Overall** | **50%** | **65%** | **85%** |

**Conclusion**: Hybrid approach (rules + examples + validation) can achieve **85%+ success rate** with 1-3 iterations.

### 10.3 Recommendation

**Proceed with the hybrid approach:**

1. ✅ Draft a concise rule prompt (15-20 critical rules)
2. ✅ Add 5-7 few-shot examples
3. ✅ Implement validation feedback loop
4. ✅ Test with real requirements
5. ✅ Iterate on prompt based on failure patterns

**Next steps:**
1. Draft the core rule prompt
2. Select representative few-shot examples
3. Define the validation feedback format
4. Build a prototype and test

---

## 11. Open Questions

1. **How many rules are "enough"?** 
   - Need empirical testing to find the sweet spot (10-30 rules?)

2. **Which examples are most effective?**
   - Should cover common patterns or edge cases?
   - How many examples before diminishing returns?

3. **How to handle lexicon words?**
   - Provide common words in prompt?
   - Allow user to specify domain lexicon?
   - Defer to validation?

4. **How many iterations are acceptable?**
   - 1-3 seems reasonable
   - Beyond that, may indicate inadequate prompt

5. **Can the LLM learn from corrections?**
   - Within a session, yes (context window)
   - Across sessions, requires fine-tuning or RAG

---

## 12. Conclusion

The **simple prompt approach is fundamentally sound** but must be **enhanced with examples and validation** to be production-ready.

**Key insight**: Don't aim for 100% correctness from the prompt alone. Instead:
- Provide **good-enough** guidance (critical rules + examples)
- **Validate** all output (parser + linter)
- **Iterate** with feedback (1-3 cycles)
- **Accept** human review for complex cases

This approach balances:
- LLM strengths (pattern recognition, natural language understanding)
- LLM weaknesses (formal logic, consistency, edge cases)
- Practical constraints (token limits, latency, cost)

**Recommendation: Proceed with hybrid implementation.**

---

**End of Evaluation**
