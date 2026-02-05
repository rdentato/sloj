# Sloj Writer Test Cases

**Version**: 1.0  
**Date**: 2026-02-05  
**Purpose**: Validate that LLMs using the writer prompt correctly generate Sloj from natural language

---

## Test Instructions

For each test case:
1. Load the Sloj Writer Prompt
2. Present the natural language input
3. Get LLM-generated Sloj output
4. Compare to expected output
5. Score: PASS (correct), PARTIAL (minor issues), FAIL (incorrect syntax or meaning)

---

## Test 1: Simple Coordination (VP)

**Input**:
"The system validates and logs the input."

**Expected Output**:
```sloj
"The system validates && logs the input."
```

**Key Learning**: Use `&&` instead of "and" for verb coordination

**Common Errors**:
- ❌ `"The system validates and logs the input."` (using word "and")
- ❌ `"The system validates & logs the input."` (wrong symbol - `&` is for nouns)

**Difficulty**: ⭐ (Easy)

---

## Test 2: Simple Coordination (NP)

**Input**:
"The admin and the user receive notifications."

**Expected Output**:
```sloj
"The admin & the user receive notifications."
```

**Key Learning**: Use `&` for noun coordination

**Common Errors**:
- ❌ `"The admin and the user receive notifications."` (using word "and")
- ❌ `"The admin && the user receive notifications."` (wrong symbol - `&&` is for verbs)

**Difficulty**: ⭐ (Easy)

---

## Test 3: Exclusive Disjunction

**Input**:
"The user is either an admin or a guest, but not both."

**Expected Output**:
```sloj
"The user is an admin / a guest."
```

**Alternative (also correct)**:
```sloj
"The user either is an admin, or is a guest."
```

**Key Learning**: Use `/` for exclusive OR (not both)

**Common Errors**:
- ❌ `"The user is an admin or a guest."` (missing exclusivity)
- ❌ `"The user is an admin & a guest."` (wrong operator - would mean both)

**Difficulty**: ⭐⭐ (Medium)

---

## Test 4: Inclusive Disjunction

**Input**:
"The system validates or logs the input, or both."

**Expected Output**:
```sloj
"The system validates &&/ logs the input."
```

**Key Learning**: Use `&&/` for inclusive OR (one or both)

**Common Errors**:
- ❌ `"The system validates or logs the input."` (using word "or")
- ❌ `"The system validates / logs the input."` (exclusive, not inclusive)

**Difficulty**: ⭐⭐ (Medium)

---

## Test 5: Modal with Scope (Requires Guillemets)

**Input**:
"The system must both validate and log the input."

**Expected Output**:
```sloj
"The system must «validate && log» the input."
```

**Key Learning**: Use guillemets to scope modals over coordinated VPs

**Common Errors**:
- ❌ `"The system must validate && log the input."` (ambiguous scope without guillemets)
- ❌ `"The system must validate and log the input."` (word "and" + no guillemets)

**Difficulty**: ⭐⭐⭐ (Hard)

---

## Test 6: Hyphenated Multi-Word Entity

**Input**:
"The system displays an error message."

**Context**: "error message" is a single concept (compound term)

**Expected Output**:
```sloj
"The system displays an error-message."
```

**Key Learning**: Hyphenate multi-word entities

**Common Errors**:
- ❌ `"The system displays an error message."` (not hyphenated)

**Note**: If context indicated TWO separate things (an error AND a message), then `"an error & a message"` would be correct. The test assumes single concept.

**Difficulty**: ⭐⭐ (Medium)

---

## Test 7: Multi-Word Proper Noun

**Input**:
"John Ford logs in to the system."

**Expected Output**:
```sloj
"John-Ford logs-in to the system."
```

**Key Learning**: Hyphenate multi-word proper nouns

**Common Errors**:
- ❌ `"John Ford logs in to the system."` (not hyphenated - would parse as two people)
- ❌ `"John Ford logs-in to the system."` (name not hyphenated)

**Difficulty**: ⭐⭐ (Medium)

---

## Test 8: Determiner on Common Noun

**Input**:
"User logs in."

**Expected Output**:
```sloj
"A user logs-in."
```

**Key Learning**: Common nouns require determiners

**Common Errors**:
- ❌ `"User logs-in."` (missing determiner)
- ❌ `"The user logs-in."` (acceptable but "a" is more common for indefinite)

**Note**: `"The user logs-in."` is also valid if referring to a specific user. The test assumes indefinite (any user).

**Difficulty**: ⭐⭐ (Medium)

---

## Test 9: No Determiner on Proper Noun

**Input**:
"The Alice logs in."

**Context**: Alice is a person's name

**Expected Output**:
```sloj
"Alice logs-in."
```

**Key Learning**: Proper nouns forbid determiners

**Common Errors**:
- ❌ `"The Alice logs-in."` (determiner on proper noun)

**Difficulty**: ⭐⭐ (Medium)

---

## Test 10: Unique Noun (With Determiner)

**Input**:
"GPS starts."

**Context**: Referring to "the GPS system" (unique in context)

**Expected Output**:
```sloj
"The GPS starts."
```

**Alternative (if GPS is a proper name)**:
```sloj
"GPS starts."
```

**Key Learning**: Unique nouns require `the`; proper nouns don't

**Common Errors**:
- Misclassifying as proper vs unique

**Note**: Without context, both are valid. Test assumes unique noun interpretation.

**Difficulty**: ⭐⭐⭐ (Hard)

---

## Test 11: Simple Conditional

**Input**:
"When the card is valid, the system accepts it."

**Expected Output**:
```sloj
"If the card is valid, then the system accepts the card."
```

**Key Learning**: Use `if...then`; use explicit noun instead of pronoun when possible

**Common Errors**:
- ❌ `"When the card is valid, the system accepts it."` (using "when" instead of "if...then")
- Using "it" is acceptable but less preferred

**Difficulty**: ⭐⭐ (Medium)

---

## Test 12: Present Perfect in Conditional

**Input**:
"If a user has logged in, the system shows the dashboard."

**Expected Output**:
```sloj
"If a user has logged-in, then the system displays the dashboard."
```

**Key Learning**: Present perfect allowed in conditional antecedent; hyphenate "logged-in"

**Common Errors**:
- ❌ `"If a user has logged in, then the system displays the dashboard."` (not hyphenating "logged-in")
- ❌ `"If a user logged in, then..."` (using simple past instead of present perfect)

**Difficulty**: ⭐⭐⭐ (Hard)

---

## Test 13: Future Forbidden in Condition

**Input**:
"If the user will log in, the system will display the dashboard."

**Expected Output**:
```sloj
"If the user logs-in, then the system will display the dashboard."
```

**Key Learning**: Use present tense in condition; future allowed in consequence

**Common Errors**:
- ❌ `"If the user will log-in, then the system will display the dashboard."` (future in condition)

**Difficulty**: ⭐⭐⭐ (Hard)

---

## Test 14: Inline List (Exclusive)

**Input**:
"The status can be active, pending, or failed (only one at a time)."

**Expected Output**:
```sloj
"The status is one of: active, pending, failed."
```

**Key Learning**: Use `one of:` for exclusive selection from list

**Common Errors**:
- ❌ `"The status is active, pending, or failed."` (not using list syntax)
- ❌ `"The status is any of: active, pending, failed."` (wrong keyword - "any of" is inclusive)

**Difficulty**: ⭐⭐ (Medium)

---

## Test 15: Inline List (Inclusive)

**Input**:
"The system performs validation, logging, or notification (one or more)."

**Expected Output**:
```sloj
"The system performs any of: validation, logging, notification."
```

**Alternative**:
```sloj
"The system validates &&/ logs &&/ notifies."
```

**Key Learning**: Use `any of:` for inclusive selection

**Difficulty**: ⭐⭐ (Medium)

---

## Test 16: Sequential Conjunction

**Input**:
"The system validates the input first, then logs the result."

**Expected Output**:
```sloj
"The system validates the input && then logs the result."
```

**Key Learning**: Use `&& then` for sequential (ordered) actions

**Common Errors**:
- ❌ `"The system validates the input && logs the result."` (missing "then" - order not specified)

**Difficulty**: ⭐⭐ (Medium)

---

## Test 17: VP Negation

**Input**:
"The server does not respond."

**Expected Output**:
```sloj
"The server does not respond."
```

**Alternative (contracted)**:
```sloj
"The server doesn't respond."
```

**Key Learning**: VP negation uses `does not` / `do not`

**Common Errors**:
- ❌ `"The server not responds."` (incorrect syntax)

**Difficulty**: ⭐ (Easy)

---

## Test 18: Modal Prohibition

**Input**:
"The system must not store passwords in plaintext."

**Expected Output**:
```sloj
"The system must not store passwords in plaintext."
```

**Alternative (with hyphenation if "plaintext" is one concept)**:
```sloj
"The system must not store passwords in plain-text."
```

**Key Learning**: Use `must not` for prohibition

**Common Errors**:
- ❌ `"The system does not must store..."` (combining negation with modal)

**Difficulty**: ⭐⭐ (Medium)

---

## Test 19: Passive Voice

**Input**:
"The card is validated by the system."

**Expected Output**:
```sloj
"The card is validated by the system."
```

**Key Learning**: Passive uses `is/are` + past participle

**Note**: This is already in Sloj-valid form. LLM should recognize and preserve it.

**Difficulty**: ⭐ (Easy)

---

## Test 20: Existential

**Input**:
"There is a valid token."

**Expected Output**:
```sloj
"There is a valid token."
```

**Key Learning**: Existential uses `there is/are`

**Note**: Already in valid form.

**Difficulty**: ⭐ (Easy)

---

## Test 21: Sentence-Level Coordination

**Input**:
"The user logs in, and the system starts."

**Expected Output**:
```sloj
"A user logs-in, and the system starts."
```

**Key Learning**: Sentence-level coordination uses `, and` (words, not symbols)

**Common Errors**:
- ❌ `"A user logs-in && the system starts."` (using `&&` between sentences)

**Difficulty**: ⭐⭐ (Medium)

---

## Test 22: Sentence-Level Exclusive Disjunction

**Input**:
"Either the user authenticates or the session expires, but not both."

**Expected Output**:
```sloj
"Either the user authenticates, or the session expires."
```

**Alternative (also correct)**:
```sloj
"Either a user authenticates, or the session expires."
```

**Key Learning**: Sentence-level exclusive uses `Either S, or S.`

**Common Errors**:
- ❌ `"The user authenticates, or the session expires."` (not explicitly exclusive)

**Difficulty**: ⭐⭐⭐ (Hard)

---

## Test 23: Reflexive Verb

**Input**:
"Bob authenticates himself."

**Expected Output**:
```sloj
"Bob self-authenticates."
```

**Key Learning**: Reflexive uses `self-` prefix, not reflexive pronouns

**Common Errors**:
- ❌ `"Bob authenticates himself."` (reflexive pronouns forbidden)

**Difficulty**: ⭐⭐ (Medium)

---

## Test 24: String Literal

**Input**:
"The status is set to 'active'."

**Expected Output**:
```sloj
"The status is set to \"active\"."
```

**Alternative (using backticks)**:
```sloj
"The status is set to `active`."
```

**Key Learning**: Use double quotes or backticks for string literals

**Common Errors**:
- ❌ `"The status is set to active."` (not quoted - would parse as adjective)
- ❌ `"The status is set to 'active'."` (single quotes not standard - use double or backticks)

**Difficulty**: ⭐⭐ (Medium)

---

## Test 25: Adverb Scope

**Input**:
"The system quickly validates and logs the input" (both actions are quick)

**Expected Output**:
```sloj
"The system quickly «validates && logs» the input."
```

**Key Learning**: Use guillemets to scope adverbs over multiple VPs

**Common Errors**:
- ❌ `"The system quickly validates && logs the input."` (adverb only applies to first VP)

**Difficulty**: ⭐⭐⭐ (Hard)

---

## Test 26: Complex Integration

**Input**:
"If the request is valid, the system must validate and log it, and the user gets a confirmation. Otherwise, the system rejects the request."

**Expected Output**:
```sloj
"If the request is valid, then the system must «validate && log» the request, and the user receives a confirmation. Otherwise, the system rejects the request."
```

**Key Learning**: Multiple concepts - conditionals, modal scope, sentence coordination, otherwise

**Common Errors**:
- Missing guillemets for modal scope
- Not hyphenating multi-word entities
- Using pronouns instead of explicit nouns

**Difficulty**: ⭐⭐⭐⭐⭐ (Extreme)

---

## Test 27: Measurement Phrase

**Input**:
"Five hundred megabytes of data are cached."

**Expected Output**:
```sloj
"500 megabytes of data are cached."
```

**Key Learning**: Measurement phrases use number + unit + `of` + mass noun

**Common Errors**:
- ❌ `"500 megabyte of data are cached."` (unit should be plural)
- ❌ `"500 megabytes of data is cached."` (verb should be plural - agrees with number)

**Difficulty**: ⭐⭐⭐ (Hard)

---

## Test 28: Purpose Clause

**Input**:
"The system logs all operations to ensure auditability."

**Expected Output**:
```sloj
"The system logs all operations, in order to ensure auditability."
```

**Key Learning**: Purpose clauses use `, in order to` + infinitive VP

**Common Errors**:
- ❌ `"The system logs all operations to ensure auditability."` (missing "in order")

**Note**: While the input form is acceptable in English, Sloj formalizes it with "in order to".

**Difficulty**: ⭐⭐ (Medium)

---

## Test 29: Annotation

**Input**:
"(ID: REQ-001) The system must validate all inputs."

**Expected Output**:
```sloj
[[ id: REQ-001 ]]
The system must validate all inputs.
```

**Key Learning**: Annotations use `[[ ]]` syntax, appear on separate line

**Difficulty**: ⭐⭐ (Medium)

---

## Test 30: Open List

**Input**:
"The status can be active, pending, failed, or other values."

**Expected Output**:
```sloj
"The status is one of: active, pending, failed, ..."
```

**Key Learning**: Use ellipsis for open (non-exhaustive) lists

**Common Errors**:
- ❌ `"The status is one of: active, pending, failed."` (missing ellipsis - list would be exhaustive)

**Difficulty**: ⭐⭐⭐ (Hard)

---

## Scoring Rubric

For each test case, score the LLM output:

- **PASS**: Output is syntactically valid Sloj and semantically equivalent to input
- **PARTIAL**: Minor issues (e.g., missing hyphen, suboptimal determiner choice) but mostly correct
- **FAIL**: Syntax errors, wrong meaning, or major violations of Sloj rules

**Target scores**:
- Easy (⭐): 95%+ PASS
- Medium (⭐⭐): 85%+ PASS
- Hard (⭐⭐⭐): 75%+ PASS
- Very Hard (⭐⭐⭐⭐): 60%+ PASS
- Extreme (⭐⭐⭐⭐⭐): 50%+ PASS

**Overall target**: 80%+ PASS across all tests (may require 1-3 iterations with validation feedback)

---

## Test Coverage Matrix

| Syntax Element | Test Cases | Difficulty |
|----------------|------------|------------|
| `&&` (VP) | 1, 16, 25 | Easy-Medium |
| `&` (NP) | 2 | Easy |
| `/` (exclusive) | 3 | Medium |
| `&&/` (inclusive) | 4, 15 | Medium |
| Guillemets (scope) | 5, 25 | Hard |
| Hyphens (entities) | 6 | Medium |
| Hyphens (names) | 7 | Medium |
| Determiners (common) | 8 | Medium |
| Determiners (proper) | 9 | Medium |
| Determiners (unique) | 10 | Hard |
| Conditionals | 11, 13, 26 | Medium-Extreme |
| Present perfect | 12 | Hard |
| Lists | 14, 15, 30 | Medium-Hard |
| Sequential | 16 | Medium |
| Negation | 17, 18 | Easy-Medium |
| Passive | 19 | Easy |
| Existential | 20 | Easy |
| Sentence coordination | 21, 22 | Medium-Hard |
| Reflexive | 23 | Medium |
| String literals | 24 | Medium |
| Measurement phrases | 27 | Hard |
| Purpose clauses | 28 | Medium |
| Annotations | 29 | Medium |
| Integration | 26 | Extreme |

---

## Validation Strategy

### Step 1: Generate Sloj
Use writer prompt to convert natural language to Sloj

### Step 2: Syntax Validation
Run generated Sloj through parser
- PASS → proceed to Step 3
- FAIL → provide error feedback to LLM, retry (max 3 iterations)

### Step 3: Semantic Check
Compare generated Sloj to expected output
- Exact match → PASS
- Equivalent meaning, different syntax → PASS
- Meaning differs → FAIL

### Step 4: Iteration (if needed)
If FAIL at Step 2 or 3:
1. Provide feedback: "Error: [specific issue]"
2. LLM retries with correction
3. Repeat validation
4. Max 3 iterations per test

---

## Usage Instructions

### Manual Testing
1. Copy writer prompt into LLM
2. Present each input
3. Collect generated output
4. Validate against expected output
5. Score using rubric

### Automated Testing
```python
# Pseudocode
for test in test_cases:
    output = llm.generate(writer_prompt + test.input)
    
    # Syntax validation (parser)
    is_valid = parser.validate(output)
    
    # Semantic validation
    is_equivalent = compare(output, test.expected_output)
    
    if is_valid and is_equivalent:
        results.append("PASS")
    else:
        # Retry with feedback
        feedback = generate_feedback(output, test.expected_output)
        retry_output = llm.generate(writer_prompt + test.input + feedback)
        # Re-validate...

overall_pass_rate = results.count("PASS") / len(results)
```

---

**End of Writer Test Cases**
