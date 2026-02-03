# Evaluation: Purpose Clauses Addition

**Date**: 2026-02-01  
**Task**: 2.15 (Purpose Clauses)  
**Status**: ✅ Complete

---

## Overview

Added purpose clause support to Sloj, enabling requirements specifications to include **non-normative** explanations of intent, rationale, or purpose behind requirements. Purpose clauses are documentary elements that do not change the formal logic mapping.

---

## 1. Purpose Clauses (Task 2.15)

### Syntax

Purpose clauses are introduced by the fixed keyword phrase `in order to` and use an infinitive VP (base verb form with no explicit subject).

**Three forms:**

1. **Prefix form**: `In order to` PurposeVP `,` S `.`
2. **Suffix form**: S `, in order to` PurposeVP `.`
3. **Incidental form**: S₁ `, in order to` PurposeVP `,` S₂ `.`

### Examples

**Prefix form**:
```
In order to comply with regulations, the lever must be locked.
In order to ensure data integrity, the system must validate all inputs.
```

**Suffix form**:
```
The system must log all operations, in order to ensure auditability.
The application must encrypt all data, in order to protect user privacy.
```

**Incidental form**:
```
The system must log all operations, in order to ensure auditability, and it must store the logs for 90 days.
The application must encrypt data, in order to protect privacy, and it must use AES-256.
```

### Design Rationale

1. **Non-normative**: Purpose clauses do NOT create requirements or change formal logic
2. **Documentary**: They explain **why** a requirement exists, not **what** it is
3. **Traceability**: Tools may retain them for analysis and documentation
4. **Readability**: Improves understanding of requirement rationale
5. **Separation of concerns**: Keeps "what" (normative) separate from "why" (documentary)

### Grammar Updates

**File**: `spec/sloj-spec-grammar.bnf`

Added to `Sentence` production:
```bnf
Sentence ::= ... | PurposeSent '.'
```

Added new productions:
```bnf
PurposeSent       ::= 'In order to' PurposeVP ',' DisjunctSent '.'
                    | DisjunctSent ', in order to' PurposeVP '.'
                    | ConjunctSent ', in order to' PurposeVP ',' SimpleSent '.'

PurposeVP         ::= InfinitiveVP

InfinitiveVP      ::= BaseVerb VerbArg? NonOfPrepPhrase*
                    | 'be' VerbArg NonOfPrepPhrase*

BaseVerb          ::= <base_verb_form>
```

---

## 2. Specification Updates

**File**: `spec/sloj-spec-doc.md`

### Section 10: Purpose Clauses

Added comprehensive documentation:
- Three forms (prefix, suffix, incidental) with examples
- Rules table covering all constraints
- Semantics section explaining non-normative nature
- Clear example showing separation of requirement from purpose

**Key rules documented:**

| Rule | Description |
|------|-------------|
| **Non-normative** | Purpose clauses do NOT change the formal logic mapping |
| **Infinitive VP** | PurposeVP must be base verb form with no explicit subject |
| **No commas** | Purpose clause must NOT contain a comma token |
| **Comma delimited** | Comma delimiting is mandatory |
| **Documentation only** | Express intent/rationale, NOT requirements |

---

## 3. Examples File Updates

**File**: `spec/sloj-examples.md`

### Section 29: Purpose Clauses

Added comprehensive examples demonstrating:
- 29.1 Prefix form (4 examples)
- 29.2 Suffix form (4 examples)
- 29.3 Incidental form (3 examples)
- 29.4 Purpose clauses with modals (4 examples)
- 29.5 Purpose clauses with conditionals (3 examples)
- 29.6 Complex purpose VPs (4 examples)
- 29.7 Non-normative nature (explanation with example)

### Quick Reference Index

Added entries:
- Purpose clause prefix
- Purpose clause suffix
- Purpose clause incidental
- Purpose clause non-normative

### Table of Contents

- Inserted section 29 with 7 subsections
- All references properly linked

---

## 4. Key Design Decisions

### 4.1 Non-Normative Status

**Decision**: Purpose clauses are documentary only and do NOT participate in formal logic mapping

**Rationale**:
- Separates "what" (requirements) from "why" (rationale)
- Prevents accidental creation of additional requirements
- Allows tools to optionally retain or discard purpose information
- Maintains clarity about what is normative vs. documentary

**Example**:
```
The system must encrypt all data, in order to protect user privacy.
```
- **Requirement**: "The system must encrypt all data" (normative)
- **Purpose**: "to protect user privacy" (documentary)

The purpose does NOT create a requirement to "protect user privacy" — it only explains why encryption is required.

### 4.2 Infinitive VP Form

**Decision**: PurposeVP must be an infinitive VP (base verb form) with no explicit subject

**Rationale**:
- Matches natural English purpose clause syntax
- Avoids subject agreement issues
- Clear and unambiguous structure
- Consistent with "in order to" idiom

**Examples**:
- ✅ `in order to ensure auditability`
- ✅ `in order to protect user privacy`
- ✅ `in order to prevent unauthorized access`
- ❌ `in order to the system ensures auditability` (has explicit subject)
- ❌ `in order to ensuring auditability` (not infinitive)

### 4.3 No Commas in Purpose VP

**Decision**: Purpose clause must NOT contain a comma token

**Rationale**:
- Simplifies parsing (comma marks end of purpose clause)
- Prevents ambiguity about clause boundaries
- Encourages concise purpose statements
- Consistent with Sngl design

**Examples**:
- ✅ `in order to ensure data integrity`
- ✅ `in order to protect user privacy`
- ❌ `in order to ensure data integrity, and maintain security` (contains comma)

If multiple purposes are needed, use coordination within the VP or create separate sentences.

### 4.4 Three Forms

**Decision**: Support prefix, suffix, and incidental forms

**Rationale**:
- **Prefix**: Emphasizes purpose before stating requirement
- **Suffix**: States requirement first, then explains rationale
- **Incidental**: Allows purpose to connect two related requirements
- Provides flexibility for different documentation styles
- Matches natural English usage patterns

### 4.5 Mandatory Comma Delimiting

**Decision**: Commas around purpose clauses are mandatory

**Rationale**:
- Clear visual separation from main sentence
- Unambiguous parsing boundaries
- Consistent with Sloj's use of commas for sentence coordination
- Prevents attachment ambiguities

---

## 5. Integration with Existing Features

### 5.1 Sentence Coordination

Purpose clauses work with sentence coordination:

```
The system must log operations, in order to ensure auditability, and it must store logs for 90 days.
```

The purpose clause appears between two coordinated sentences (incidental form).

### 5.2 Conditionals

Purpose clauses can appear with conditionals:

```
In order to prevent data loss, if the connection fails then the system must retry.
If the user is authenticated, in order to track activity, the system must log the session.
```

### 5.3 Modals

Purpose clauses work naturally with deontic modals:

```
In order to maintain performance, the cache must be cleared every hour.
The system must not log passwords, in order to protect user credentials.
```

### 5.4 Temporal Statements

Purpose clauses can combine with temporal/future statements:

```
In order to ensure reliability, the backup will run daily.
The system will encrypt data, in order to protect privacy.
```

---

## 6. Completeness Check

### Specification Document ✅
- [x] Section 10: Purpose Clauses
- [x] Three forms documented with examples
- [x] Rules table with all constraints
- [x] Semantics section explaining non-normative nature
- [x] Clear example showing requirement vs. purpose

### Grammar File ✅
- [x] `PurposeSent` production (3 forms)
- [x] `PurposeVP` production
- [x] `InfinitiveVP` production
- [x] `BaseVerb` terminal
- [x] Integration with `Sentence` production
- [x] Comments explaining non-normative nature

### Examples File ✅
- [x] Section 29: Purpose Clauses
- [x] 29.1: Prefix form (4 examples)
- [x] 29.2: Suffix form (4 examples)
- [x] 29.3: Incidental form (3 examples)
- [x] 29.4: With modals (4 examples)
- [x] 29.5: With conditionals (3 examples)
- [x] 29.6: Complex VPs (4 examples)
- [x] 29.7: Non-normative nature explanation
- [x] Quick Reference Index updated
- [x] Table of Contents updated

### Project Management ⏳
- [ ] PLAN.md updated (pending)
- [ ] NOTES.md updated (pending)

---

## 7. Testing Considerations

### Valid Constructs

**Prefix form**:
```
In order to comply with regulations, the lever must be locked.
In order to ensure data integrity, the system must validate all inputs.
```

**Suffix form**:
```
The system must log all operations, in order to ensure auditability.
The application must encrypt all data, in order to protect user privacy.
```

**Incidental form**:
```
The system must log operations, in order to ensure auditability, and it must store logs for 90 days.
```

**With conditionals**:
```
In order to prevent data loss, if the connection fails then the system must retry.
```

### Invalid Constructs

**Missing commas**:
```
❌ In order to ensure auditability the system must log operations.
❌ The system must log operations in order to ensure auditability.
```

**Comma in purpose VP**:
```
❌ The system must log operations, in order to ensure auditability, and maintain security.
```
(The comma after "auditability" makes it ambiguous whether "and maintain security" is part of the purpose or a new sentence)

**Non-infinitive VP**:
```
❌ The system must log operations, in order to ensuring auditability.
❌ The system must log operations, in order to it ensures auditability.
```

**Explicit subject in purpose VP**:
```
❌ The system must log operations, in order to the system ensures auditability.
```

---

## 8. Semantic Interpretation

### Formal Logic Mapping

Purpose clauses are **transparent** to formal logic mapping:

```
The system must encrypt all data, in order to protect user privacy.
```

**Formal interpretation**:
```
must(encrypt(system, all_data))
```

The purpose clause "in order to protect user privacy" is **not** included in the formal logic. It is retained as metadata/documentation only.

### Tool Behavior

**Parsers**:
- MUST recognize purpose clause syntax
- MUST validate infinitive VP structure
- MUST enforce comma delimiting rules
- MAY retain purpose text as metadata

**Logic translators**:
- MUST ignore purpose clauses in formal logic mapping
- MAY attach purpose as annotation/comment to requirement

**Documentation generators**:
- MAY include purpose clauses in generated documentation
- MAY use purpose for traceability matrices
- MAY highlight purpose for human readers

---

## 9. Comparison with Sngl

Sloj's purpose clauses are **identical** to Sngl's purpose clauses:

| Aspect | Sngl | Sloj |
|--------|------|------|
| Keyword | `in order to` | `in order to` ✅ |
| Forms | Prefix, suffix, incidental | Prefix, suffix, incidental ✅ |
| VP type | Infinitive | Infinitive ✅ |
| Comma delimiting | Mandatory | Mandatory ✅ |
| Non-normative | Yes | Yes ✅ |
| No commas in VP | Yes | Yes ✅ |

**Rationale for keeping identical**:
- Purpose clauses are already well-designed in Sngl
- No conflicts with Sloj's symbolic coordination (purpose clauses are sentence-level)
- Non-normative nature means they don't affect formal semantics
- Natural English idiom that doesn't need modification

---

## 10. Future Considerations

### Potential Extensions

1. **Alternative keywords**: Consider `so that`, `to`, `for` as alternatives to `in order to`
   - Currently NOT in scope (would require careful semantic analysis)

2. **Nested purposes**: Allow purpose clauses within purpose clauses
   - Currently NOT supported (no commas allowed in purpose VP)

3. **Purpose with VP lists**: Allow purpose clauses with VP list coordination
   - Currently NOT explicitly addressed

### Not in Scope

These features are explicitly **not** part of Sloj:
- Purpose clauses as formal requirements
- Purpose clauses creating logical implications
- Purpose clauses with quantifiers or complex logic

---

## 11. Documentation Impact

### For Requirements Writers

Purpose clauses provide a way to:
- Explain **why** requirements exist
- Document rationale for design decisions
- Improve traceability to business goals
- Make specifications more understandable

**Best practices**:
- Use purpose clauses sparingly (only when rationale is not obvious)
- Keep purpose VPs concise (no commas)
- Don't use purpose clauses to state additional requirements
- Consider whether purpose belongs in requirement or separate documentation

### For Requirements Readers

Purpose clauses help readers:
- Understand intent behind requirements
- Make informed decisions about implementation
- Identify requirements that may need updating when context changes
- Distinguish normative requirements from explanatory rationale

---

## 12. Summary

Successfully added purpose clause support to Sloj, providing:

1. **Three forms** (prefix, suffix, incidental)
   - Flexible placement for different documentation styles
   - Natural English syntax with `in order to`

2. **Non-normative semantics**
   - Clear separation of requirements from rationale
   - Transparent to formal logic mapping
   - Optional retention by tools

3. **Complete documentation**
   - Specification document updated (Section 10)
   - Grammar file updated with new productions
   - Examples file updated with comprehensive examples (Section 29)

4. **Consistent design**
   - Identical to Sngl (no modifications needed)
   - Integrates with existing Sloj features
   - Clear rules and constraints

**Task 2.15 is now complete.**
