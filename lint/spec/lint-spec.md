# Sloj Linter Specification

**Version**: 0.1  
**Status**: Draft  
**Date**: 2026-02-05

---

## 1. Overview

The Sloj linter is a semantic checker for Sloj documents.

The linter validates semantic correctness beyond syntax.

The linter detects errors that pass syntax checking.

---

## 2. Scope and Purpose

### 2.1 What the Linter Does

The linter performs semantic analysis.

The linter validates reference resolution.

The linter checks type compatibility.

The linter detects logical inconsistencies.

The linter enforces style conventions.

### 2.2 What the Linter Does Not Do

The linter does not parse Sloj syntax.

The linter does not generate formal logic mappings.

The linter does not execute requirements.

The linter does not modify input documents.

---

## 3. Input and Output

### 3.1 Input

The linter accepts a Sloj document.

The linter accepts a lexicon-file.

The linter accepts a configuration-file.

### 3.2 Output

The linter produces a report.

The report contains one of: zero errors, one error, multiple errors.

Each error has a severity-level.

Each error has a location.

Each error has a message.

Each error may have a suggestion.

---

## 4. Semantic Check Categories

### 4.1 Agreement Checks

**Purpose**: The system ensures that linguistic elements agree in number, gender, and person.

#### 4.1.1 Noun-Verb Agreement

The linter checks noun-verb agreement.

If a subject is singular, then the verb must be singular.

If a subject is plural, then the verb must be plural.

The linter reports an error if agreement is violated.

**Examples of violations:**

- "The dog run." (singular subject with plural verb)
- "The dogs runs." (plural subject with singular verb)

#### 4.1.2 Determiner-Noun Agreement

The linter checks determiner-noun compatibility.

A mass determiner must not precede a count-noun.

A count determiner must not precede a mass-noun.

The determiner "a" must not precede a plural noun.

The determiner "enough" must precede a plural noun, or the determiner "enough" must precede a mass-noun.

#### 4.1.3 Pronoun-Antecedent Agreement

The linter checks pronoun-antecedent agreement.

If a pronoun is singular, then the antecedent must be singular.

If a pronoun is plural, then the antecedent must be plural.

The pronoun gender must match the antecedent gender.

### 4.2 Reference Resolution Checks

**Purpose**: The system ensures that all references have valid antecedents.

#### 4.2.1 Pronoun Antecedent Resolution

The linter validates pronoun resolution.

Every pronoun must have an antecedent.

The antecedent must precede the pronoun.

The antecedent must match the pronoun in gender.

The antecedent must match the pronoun in number.

The string-literals are invisible to pronoun resolution.

The code-blocks are invisible to pronoun resolution.

#### 4.2.2 Binding Validation

The linter validates proper-noun bindings.

A proper-noun binding must bind to one of: a common-noun, a unique-noun, a mass-noun.

The bound proper-noun must not appear before the binding point.

#### 4.2.3 Relative Clause Antecedent

The linter validates relative-clause antecedents.

Every relative-clause must modify a noun-phrase.

The modified noun-phrase immediately precedes the relative-clause.

### 4.3 Scope Validation Checks

**Purpose**: The system ensures that modifiers and operators have proper scope.

#### 4.3.1 Modal Scope

The linter validates modal-scope.

A modal attaches to the closest verb.

If a modal must scope over coordinated verbs, then guillemets must delimit the coordinated verbs.

#### 4.3.2 Negation Scope

The linter validates negation-scope.

The VP-negation attaches to the closest verb.

If negation must scope over coordinated verbs, then guillemets must delimit the coordinated verbs.

The VP-negation must not combine with modal auxiliaries.

#### 4.3.3 Adverb Scope

The linter validates adverb-scope.

An adverb in a simple VP modifies only that VP.

If an adverb must modify a guillemeted VP group, then the adverb must immediately precede the guillemets, or the adverb must immediately follow the guillemets.

At most one adverb-phrase may appear per VP.

### 4.4 Temporal Logic Checks

**Purpose**: The system ensures temporal constructs are used correctly.

#### 4.4.1 Present Perfect Usage

The linter validates present-perfect usage.

The present-perfect is permitted in conditional antecedents.

The present-perfect is permitted in relative-clauses.

The present-perfect is forbidden in conditional consequences.

The present-perfect is forbidden in simple sentences.

#### 4.4.2 Future Tense Restrictions

The linter validates future-tense usage.

The future-tense is forbidden in conditional antecedents.

The future-tense is forbidden in temporal prefix conditions.

The future-tense is permitted in conditional consequences.

The future-tense is permitted in simple sentences.

#### 4.4.3 Temporal Sequence Consistency

The linter validates temporal sequences.

A temporal sequence must use "first", "then", and optionally "finally".

The sequence "first...then...finally" must appear in that order.

### 4.5 Type Compatibility Checks

**Purpose**: The system ensures semantic type consistency.

#### 4.5.1 Verb Argument Compatibility

The linter validates verb-argument compatibility.

The linter consults the lexicon.

If a verb requires an NP argument, then the argument must be an NP.

If a verb requires an Adj argument, then the argument must be an Adj.

If a verb requires a PP argument, then the argument must be a PP.

The linter reports an error if argument type is incompatible.

#### 4.5.2 Comparison Type Checking

The linter validates comparison types.

A comparative adjective must be followed by "than" and a comparand.

A superlative adjective must be preceded by "the".

The comparand type must be compatible with the comparison context.

#### 4.5.3 Measurement Phrase Validation

The linter validates measurement phrases.

A measurement-phrase has the form: Number Unit "of" MassNoun.

The unit must be in the lexicon.

If the number is 1, then the unit must be singular.

If the number is not equal to 1, then the unit must be plural.

### 4.6 Partitive Validation Checks

**Purpose**: The system ensures partitives are well-formed.

#### 4.6.1 Partitive Structure

The linter validates partitive structure.

A proportion partitive must use "of the".

A count partitive must use "of the".

The complement must be a definite noun-phrase.

#### 4.6.2 Partitive Agreement

The linter validates partitive agreement.

If the partitive head is singular, then the verb must be singular.

If the partitive head is plural, then the verb must be plural.

### 4.7 List Validation Checks

**Purpose**: The system ensures lists are well-formed and semantically valid.

#### 4.7.1 List Structure

The linter validates list structure.

A list must have at least one item.

A list must use a list keyword.

The list keyword must be one of: "one of", "any of", "all of", "none of".

A markdown-list must be the final element of a sentence.

#### 4.7.2 List Coordination Semantics

The linter validates list coordination.

If a list uses "one of", then exactly one item holds.

If a list uses "any of", then at least one item holds.

If a list uses "all of", then all items hold.

If a list uses "none of", then no items hold.

#### 4.7.3 Ellipsis Validation

The linter validates ellipsis usage.

If a list contains an ellipsis, then the ellipsis must be the final element.

The ellipsis indicates an open list.

If a list does not contain an ellipsis, then the list is exhaustive.

### 4.8 String Literal Checks

**Purpose**: The system ensures string literals are used correctly.

#### 4.8.1 String Invisibility

The linter validates string-literal invisibility.

The string-literals are invisible to pronoun resolution.

The linter skips string-literals when resolving pronouns.

#### 4.8.2 String Escaping

The linter validates string-literal escaping.

A double-quoted string must escape internal double quotes with backslash.

A backtick string must escape internal backticks with backslash.

Backslashes must be escaped with backslash.

### 4.9 Prepositional Phrase Attachment Checks

**Purpose**: The system ensures PPs attach correctly.

#### 4.9.1 Of-Clause Attachment

The linter validates of-clause attachment.

An of-clause always binds to the preceding noun-phrase.

An of-clause must not bind to a verb.

#### 4.9.2 Non-Of PP Attachment

The linter validates non-of PP attachment.

If a preceding verb exists, then the non-of PP binds to the verb.

If no preceding verb exists, then the non-of PP binds to the preceding noun.

If a noun-phrase is in object position, then the noun-phrase must not contain a non-of PP unless guillemets delimit the noun-phrase.

### 4.10 Exception Validation Checks

**Purpose**: The system ensures exception constructs are valid.

#### 4.10.1 Exception Suffix Structure

The linter validates exception-suffix structure.

An exception-suffix uses "except" followed by a noun-phrase.

An exception-suffix may use "except the one that" VP.

An exception-suffix may use "except the ones that" VP.

#### 4.10.2 Exception in Conditionals

The linter validates exception conditionals.

An exception conditional uses "except when" or "except if".

The exception condition must use present tense or present-perfect.

---

## 5. Consistency and Contradiction Checks

### 5.1 Contradiction Detection

The linter detects contradictions.

If a sentence states P, and another sentence states not P, then the linter reports a potential contradiction.

The linter uses basic logical inference.

### 5.2 Redundancy Detection

The linter detects redundancy.

If two sentences express the same requirement, then the linter reports potential redundancy.

### 5.3 Tautology Detection

The linter detects tautologies.

If a sentence is always true, then the linter reports a potential tautology.

---

## 6. Style and Best Practice Checks

### 6.1 Terminology Consistency

The linter checks terminology consistency.

If a concept is referred to by multiple terms, then the linter reports inconsistent terminology.

The linter suggests using a consistent term.

### 6.2 Preferred Constructions

The linter suggests preferred constructions.

If a sentence uses a discouraged pattern, then the linter suggests an alternative.

### 6.3 Readability Metrics

The linter computes readability metrics.

The linter measures sentence length.

The linter measures nesting depth.

If a sentence is too complex, then the linter suggests simplification.

### 6.4 Guillemet Usage

The linter validates guillemet usage.

Guillemets must be balanced.

Guillemets must delimit well-formed phrases.

If guillemets are unnecessary, then the linter suggests removal.

---

## 7. Annotation Validation

### 7.1 Annotation Structure

The linter validates annotation structure.

An annotation must use the syntax "[[" content "]]".

An annotation must appear on its own line.

An annotation must immediately precede the element it annotates.

### 7.2 Annotation Content

The linter validates annotation content.

If an annotation uses key-value pairs, then each pair must have the form "key: value".

Multiple key-value-pairs must be separated by commas.

---

## 8. Lexicon Validation

### 8.1 Lexicon Checking

The linter validates lexicon usage.

Every noun must be in the lexicon.

Every verb must be in the lexicon.

Every adjective must be in the lexicon.

Every adverb must be in the lexicon.

If a word is not in the lexicon, then the linter reports an unknown word error.

### 8.2 Lexicon Categories

The linter validates lexicon categories.

A noun in the lexicon has a category.

The category is one of: common, unique, mass, proper.

A verb in the lexicon has argument requirements.

The linter checks that verb arguments match lexicon requirements.

---

## 9. Error Reporting

### 9.1 Error Format

An error has a severity.

The severity is one of: error, warning, info.

An error has a location.

The location specifies the file, line-number, and column-number.

An error has a message.

The message describes the problem.

An error may have a suggestion.

The suggestion describes how to fix the problem.

### 9.2 Severity Levels

**Error**: The document violates a semantic rule. The document must be corrected.

**Warning**: The document may have a problem. The user should review the flagged construct.

**Info**: The linter provides information. The document is valid but could be improved.

### 9.3 Error Messages

An error-message must be clear.

An error-message must be actionable.

An error-message must cite the specific rule violated.

An error-message should provide context.

---

## 10. Configuration

### 10.1 Configuration File

The linter accepts a configuration-file.

The configuration-file specifies enabled checks.

The configuration-file specifies severity overrides.

The configuration-file specifies custom lexicon paths.

### 10.2 Check Enablement

Each check has an identifier.

The user may enable a check, or the user may disable a check.

By default, all checks are enabled.

### 10.3 Severity Overrides

The user may override the severity of a check.

If a check is normally an error, then the user may downgrade it to a warning.

If a check is normally a warning, then the user may upgrade it to an error.

---

## 11. Performance Requirements

The linter must be fast.

The linter must process documents in real-time for editor integration.

The linter must process a 1000-line document within 1 second.

The linter must use incremental analysis when possible.

---

## 12. Extensibility

### 12.1 Custom Checks

The linter architecture must support custom checks.

A user may define custom semantic rules.

A custom check must follow the standard check interface.

### 12.2 Plugin System

The linter may support a plugin system.

A plugin may add new check categories.

A plugin may extend the lexicon.

---

## 13. Integration

### 13.1 Command Line Interface

The linter must provide a CLI.

The CLI accepts a file-path.

The CLI accepts configuration options.

The CLI outputs errors to stdout or a file.

### 13.2 Language Server Protocol

The linter should support LSP.

LSP integration enables real-time checking in editors.

The linter provides diagnostics to the editor.

### 13.3 CI/CD Integration

The linter must support CI/CD integration.

The linter returns a non-zero exit-code if errors are found.

The linter outputs machine-readable formats.

The linter supports JSON output.

---

## Appendix A: Check Identifier Reference

Each check has a unique identifier.

The identifier follows the pattern: CATEGORY-NUMBER.

**Examples:**

- AGR-001: Noun-verb agreement
- AGR-002: Determiner-noun agreement
- AGR-003: Pronoun-antecedent agreement
- REF-001: Pronoun antecedent resolution
- REF-002: Binding validation
- REF-003: Relative clause antecedent
- SCP-001: Modal scope validation
- SCP-002: Negation scope validation
- SCP-003: Adverb scope validation
- TMP-001: Present perfect usage validation
- TMP-002: Future tense restrictions
- TMP-003: Temporal sequence consistency
- TYP-001: Verb argument compatibility
- TYP-002: Comparison type checking
- TYP-003: Measurement phrase validation
- PAR-001: Partitive structure validation
- PAR-002: Partitive agreement validation
- LST-001: List structure validation
- LST-002: List coordination semantics
- LST-003: Ellipsis validation
- STR-001: String literal invisibility
- STR-002: String escaping validation
- PP-001: Of-clause attachment
- PP-002: Non-of PP attachment
- EXC-001: Exception suffix structure
- EXC-002: Exception in conditionals
- CON-001: Contradiction detection
- CON-002: Redundancy detection
- CON-003: Tautology detection
- STY-001: Terminology consistency
- STY-002: Preferred constructions
- STY-003: Readability metrics
- STY-004: Guillemet usage
- ANN-001: Annotation structure
- ANN-002: Annotation content validation
- LEX-001: Lexicon word checking
- LEX-002: Lexicon category validation

---

## Appendix B: Example Error Messages

### Example 1: Noun-Verb Agreement Violation

**Location**: line 42, column 15

**Severity**: error

**Check ID**: AGR-001

**Message**: Subject-verb agreement violation. The subject "dogs" is plural, but the verb "runs" is singular.

**Suggestion**: Change "runs" to "run".

### Example 2: Unresolved Pronoun

**Location**: line 18, column 50

**Severity**: error

**Check ID**: REF-001

**Message**: Pronoun "she" has no valid antecedent. No preceding noun-phrase matches gender (female) and number (singular).

**Suggestion**: Introduce a female singular noun-phrase before this pronoun, or replace "she" with an explicit noun-phrase.

### Example 3: Present Perfect in Consequence

**Location**: line 67, column 30

**Severity**: error

**Check ID**: TMP-001

**Message**: Present-perfect is forbidden in conditional consequences. The consequence clause uses "has validated".

**Suggestion**: Change "has validated" to "validates" (present-tense) or "will validate" (future-tense).

### Example 4: Missing Guillemets for Modal Scope

**Location**: line 103, column 10

**Severity**: warning

**Check ID**: SCP-001

**Message**: Modal "must" may have ambiguous scope. The modal precedes coordinated verbs "validate && log", but guillemets are absent.

**Suggestion**: If "must" should scope over both verbs, use guillemets: "must «validate && log»".

### Example 5: Unknown Word

**Location**: line 22, column 8

**Severity**: error

**Check ID**: LEX-001

**Message**: Unknown word "valdate". The word is not in the lexicon.

**Suggestion**: Did you mean "validate"?

---

**End of Specification**
