# Sloj Reader Test Cases

**Version**: 1.0  
**Date**: 2026-02-05  
**Purpose**: Validate that LLMs using the reader prompt correctly interpret Sloj text

---

## Test Instructions

For each test case:
1. Load the Sloj Reader Prompt
2. Present the Sloj text
3. Ask the question(s)
4. Compare LLM answer to expected answer
5. Score: PASS (correct), PARTIAL (mostly correct), FAIL (incorrect)

---

## Test 1: Simple Coordination (&&)

**Sloj Text**:
```sloj
"The system validates && logs the input."
```

**Questions**:
1. What does the system do?
2. Do both actions happen?

**Expected Answers**:
1. "The system performs two actions: (1) validates the input, AND (2) logs the input."
2. "Yes, both actions occur (parallel conjunction via &&)."

**Key Learning**: Interpret `&&` as conjunction (both actions)

**Difficulty**: ⭐ (Easy)

---

## Test 2: Exclusive Disjunction (/)

**Sloj Text**:
```sloj
"The user is an admin / a guest."
```

**Questions**:
1. Can the user be both admin and guest?
2. What are the possible states?

**Expected Answers**:
1. "No. The user cannot be both simultaneously. The `/` operator indicates exclusive disjunction (exactly one, not both)."
2. "Two mutually exclusive states: (1) admin, OR (2) guest."

**Key Learning**: `/` means EXCLUSIVE OR (exactly one, not both)

**Difficulty**: ⭐⭐ (Medium) - Natural bias toward inclusive "or"

---

## Test 3: Inclusive Disjunction (&&/)

**Sloj Text**:
```sloj
"The system validates &&/ logs the input."
```

**Questions**:
1. What actions can occur?
2. Must both actions occur?

**Expected Answers**:
1. "One or both actions occur: (1) validation only, (2) logging only, OR (3) both validation and logging."
2. "No. At least one action must occur, but both is also allowed (inclusive disjunction via &&/)."

**Key Learning**: `&&/` means inclusive OR (one or both)

**Difficulty**: ⭐⭐ (Medium)

---

## Test 4: Scope with Guillemets (Modal)

**Sloj Text**:
```sloj
"The system must «validate && log» the input."
```

**Questions**:
1. What is required?
2. Does "must" apply to both actions?

**Expected Answers**:
1. "Both validation and logging are required."
2. "Yes. The guillemets scope 'must' over both VPs. Both actions are mandatory."

**Contrast (without guillemets)**:
```sloj
"The system must validate && log the input."
```
Expected: "Ambiguous. 'must' clearly applies to 'validate', but its application to 'log' is unclear without guillemets."

**Key Learning**: Guillemets control scope of modifiers

**Difficulty**: ⭐⭐⭐ (Hard) - Easy to ignore guillemets

---

## Test 5: Hyphenated Compound

**Sloj Text**:
```sloj
"The system displays the error-message."
```

**Questions**:
1. How many things are displayed?
2. What is "error-message"?

**Expected Answers**:
1. "One thing is displayed."
2. "A single compound entity: an error-message. The hyphen joins the words into a single concept."

**Key Learning**: Hyphens create single entities

**Difficulty**: ⭐⭐ (Medium)

---

## Test 6: Capitalization + Determiner (Proper vs Unique)

**Sloj Text**:
```sloj
Sentence A: "GPS starts."
Sentence B: "The GPS starts."
```

**Question**:
What's the difference between these two sentences?

**Expected Answer**:
"Sentence A: 'GPS' (capitalized without determiner) is a proper noun—a specific named entity. Sentence B: 'The GPS' (capitalized with determiner 'the') is a unique noun—a unique thing in context. The presence or absence of the determiner determines the noun category."

**Key Learning**: Determiner presence distinguishes proper from unique nouns

**Difficulty**: ⭐⭐⭐ (Hard) - Subtle distinction

---

## Test 7: Conditional with Exclusive Outcome

**Sloj Text**:
```sloj
"If a user has logged-in, then the system either displays the dashboard, or shows an error-message."
```

**Questions**:
1. What happens when a user logs in?
2. Can both dashboard and error appear?
3. What if the user hasn't logged in?

**Expected Answers**:
1. "When a user has successfully logged in (present perfect indicates prior event), exactly one of two outcomes occurs: (1) the dashboard is displayed, OR (2) an error message is shown."
2. "No. This is exclusive disjunction via 'either...or'. Only one outcome occurs, not both."
3. "If the user has not logged in, the conditional does not apply. Neither outcome is triggered."

**Key Learning**: Conditionals + exclusive disjunction + present perfect

**Difficulty**: ⭐⭐⭐⭐ (Very Hard) - Multiple concepts combined

---

## Test 8: List Semantics (Exhaustive)

**Sloj Text**:
```sloj
"The status is one of: active, pending, failed."
```

**Questions**:
1. How many statuses can the system have simultaneously?
2. Is "completed" a valid status?
3. Is the list exhaustive or open?

**Expected Answers**:
1. "Exactly one status at any time. The 'one of:' keyword indicates exclusive selection."
2. "No. Only the listed values (active, pending, failed) are valid."
3. "Exhaustive (closed). The list does not end with ellipsis, so only the listed items are valid."

**Key Learning**: `one of:` = exactly one; lists are exhaustive by default

**Difficulty**: ⭐⭐ (Medium)

---

## Test 9: List Semantics (Open)

**Sloj Text**:
```sloj
"The status is one of: active, pending, failed, ..."
```

**Questions**:
1. Is "completed" a valid status?
2. What does the ellipsis mean?

**Expected Answers**:
1. "Possibly. The ellipsis indicates an open (non-exhaustive) list, so other values beyond the listed ones may be valid."
2. "The ellipsis (`...`) indicates that the list is not exhaustive. Additional values beyond active, pending, and failed may exist."

**Key Learning**: Ellipsis makes lists open/extensible

**Difficulty**: ⭐⭐ (Medium)

---

## Test 10: Modal Distinctions

**Sloj Text**:
```sloj
Sentence A: "The system must validate the input."
Sentence B: "The system may validate the input."
Sentence C: "The system should validate the input."
```

**Question**:
What is the difference in meaning between these three sentences?

**Expected Answer**:
"Sentence A (must): Validation is mandatory/required. The system is obligated to validate. Sentence B (may): Validation is optional/permitted. The system is allowed to validate but not required. Sentence C (should): Validation is recommended/advised. The system is encouraged to validate but it's not mandatory."

**Key Learning**: Modals have formal, precise meanings

**Difficulty**: ⭐⭐ (Medium)

---

## Test 11: Present Perfect in Conditional

**Sloj Text**:
```sloj
"If a user has logged-in, then the system displays the dashboard."
```

**Questions**:
1. What does "has logged-in" indicate?
2. Is the user currently logged in?

**Expected Answers**:
1. "'Has logged-in' (present perfect) indicates that a login event occurred at some prior time-point."
2. "Not necessarily. Present perfect asserts a prior event, not current state. The user was logged in before, but may or may not currently be logged in. For current state, use present tense: 'is logged-in'."

**Key Learning**: Present perfect indicates prior event, not current state

**Difficulty**: ⭐⭐⭐ (Hard) - Subtle temporal semantics

---

## Test 12: VP Sequential Conjunction

**Sloj Text**:
```sloj
"The system validates && then logs the input."
```

**Questions**:
1. In what order do actions occur?
2. Can they happen simultaneously?

**Expected Answers**:
1. "Validation occurs first, then logging occurs second. Sequential order."
2. "No. The '&& then' operator indicates sequential (ordered) conjunction. Logging must follow validation."

**Contrast (parallel)**:
```sloj
"The system validates && logs the input."
```
Expected: "Both actions occur, but order is not specified. They may happen simultaneously or in either order (parallel conjunction via &&)."

**Key Learning**: `&& then` = sequential; `&&` = parallel

**Difficulty**: ⭐⭐ (Medium)

---

## Test 13: Sentence-Level Exclusive Disjunction

**Sloj Text**:
```sloj
"Either the user authenticates, or the session expires."
```

**Questions**:
1. Can both happen?
2. How is this different from VP-level "either...or"?

**Expected Answers**:
1. "No. Exactly one sentence holds: either authentication occurs, OR session expiration occurs, but not both (exclusive disjunction at sentence level)."
2. "This is sentence-level exclusive disjunction because 'Either' begins the sentence. VP-level 'either...or' appears after the subject within a sentence: 'The system either validates, or rejects the input.'"

**Key Learning**: Position of "either" determines scope

**Difficulty**: ⭐⭐⭐ (Hard)

---

## Test 14: Passive Voice

**Sloj Text**:
```sloj
"The card is validated by the system."
```

**Questions**:
1. What is the focus of this sentence?
2. Who performs the validation?

**Expected Answers**:
1. "The focus is on the card (what is being validated), not on who validates it. Passive voice emphasizes the patient (thing affected)."
2. "The system performs the validation. The agent is specified by the 'by' phrase."

**Contrast (active)**:
```sloj
"The system validates the card."
```
Expected: "The focus is on the system (the agent). Active voice emphasizes who performs the action."

**Key Learning**: Passive shifts focus to patient (what's affected)

**Difficulty**: ⭐⭐ (Medium)

---

## Test 15: Annotation (Non-Normative)

**Sloj Text**:
```sloj
[[ id: REQ-001, priority: high ]]
The system must validate all inputs.
```

**Questions**:
1. Does the annotation affect the requirement's meaning?
2. What is the requirement?

**Expected Answers**:
1. "No. Annotations are metadata only (non-normative). They provide information for requirements management but don't affect the formal meaning."
2. "The requirement is: The system must validate all inputs. This is a mandatory obligation."

**Key Learning**: Annotations are metadata, not normative

**Difficulty**: ⭐ (Easy)

---

## Test 16: Purpose Clause (Non-Normative)

**Sloj Text**:
```sloj
"The system must log all operations, in order to ensure auditability."
```

**Questions**:
1. What is the requirement?
2. Does "ensure auditability" create an additional requirement?

**Expected Answers**:
1. "The requirement is: The system must log all operations. This is mandatory."
2. "No. The purpose clause 'in order to ensure auditability' explains WHY logging is required (rationale), but does not create a separate requirement to ensure auditability. Purpose clauses are non-normative."

**Key Learning**: Purpose clauses explain "why", not "what"

**Difficulty**: ⭐⭐ (Medium)

---

## Test 17: Complex Requirement (Integration)

**Sloj Text**:
```sloj
"If the request is valid, then the system must «validate && log» the request, and the user receives a confirmation, or the system rejects the request."
```

**Questions**:
1. What are all the possible outcomes?
2. When does the user receive confirmation?

**Expected Answers**:
1. "If the request is valid: (1) BOTH validation and logging are required (must scopes over both via guillemets), AND the user receives confirmation, OR (2) the system rejects the request. This is Disjunctive Normal Form: (Valid ∧ Validate ∧ Log ∧ Confirm) ∨ (Valid ∧ Reject)."
2. "The user receives confirmation in the first disjunct (when the system validates and logs). If the system rejects, no confirmation is sent."

**Key Learning**: Complex integration of conditionals, modal scope, coordination

**Difficulty**: ⭐⭐⭐⭐⭐ (Extreme)

---

## Scoring Rubric

For each test case, score the LLM response:

- **PASS**: Answer is correct and demonstrates understanding of the key learning
- **PARTIAL**: Answer is mostly correct but misses some nuance or detail
- **FAIL**: Answer is incorrect or shows misunderstanding of the key concept

**Target scores**:
- Easy (⭐): 95%+ PASS
- Medium (⭐⭐): 85%+ PASS
- Hard (⭐⭐⭐): 75%+ PASS
- Very Hard (⭐⭐⭐⭐): 60%+ PASS
- Extreme (⭐⭐⭐⭐⭐): 50%+ PASS

**Overall target**: 85%+ PASS across all tests

---

## Test Coverage Matrix

| Syntax Element | Test Cases | Difficulty |
|----------------|------------|------------|
| `&&` (parallel) | 1, 12 | Easy-Medium |
| `/` (exclusive) | 2, 8 | Medium |
| `&&/` (inclusive) | 3 | Medium |
| Guillemets (scope) | 4 | Hard |
| Hyphens | 5 | Medium |
| Determiner rules | 6 | Hard |
| Conditionals | 7, 11 | Very Hard |
| Lists | 8, 9 | Medium |
| Modals | 10 | Medium |
| Present perfect | 11 | Hard |
| Sequential (`&& then`) | 12 | Medium |
| Sentence coordination | 13 | Hard |
| Passive voice | 14 | Medium |
| Annotations | 15 | Easy |
| Purpose clauses | 16 | Medium |
| Integration | 17 | Extreme |

---

## Usage Instructions

### Manual Testing
1. Copy the reader prompt into LLM
2. Present each test case
3. Record responses
4. Score using rubric
5. Calculate overall PASS rate

### Automated Testing
```python
# Pseudocode
for test in test_cases:
    response = llm.query(reader_prompt + test.sloj_text + test.questions)
    score = evaluate(response, test.expected_answers)
    results.append(score)

overall_pass_rate = sum(results) / len(results)
```

---

**End of Reader Test Cases**
