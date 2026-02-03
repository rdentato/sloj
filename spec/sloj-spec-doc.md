# Sloj Specification

**Status**: Draft  
**Version**: 0.1

**Note**: Comprehensive examples demonstrating all language constructs are available in [`sloj-examples.md`](sloj-examples.md). The examples file includes a Quick Reference Index with cross-references to this specification and the grammar file.

---

## 1. Overview

Sloj is an English-based Controlled Natural Language designed for unambiguous software requirements specifications.
It retains readable English structure, but it is deterministically parseable (every sentence has exactly one parse tree) and formally grounded (every parse tree maps to exactly one logical formula).

**Key characteristics:**

| Feature | Description |
|---------|-------------|
| Symbolic Coordination | Replaces `and`/`or` within phrases with explicit operators (`&`, `/`, `&&`), except for `either...or` and `neither...nor` |
| Explicit Scope | Uses guillemets `«...»` to delimit scope of modifiers and operators (Windows: Alt+0171 for `«`, Alt+0187 for `»`) |
| Structural Precedence | Enforces strict hierarchy of sentence construction |

---

## 2. Sentence Structure

Sloj documents are sequences of sentences. Sentence coordination is the only place where plain English conjunctions are permitted.

### 2.1 Sentence Coordination

Sentences use a two-level hierarchy enforcing Disjunctive Normal Form (DNF):

| Level | Separator | Logic |
|-------|-----------|-------|
| Disjunction (outer) | `, or` | $S_1 \lor S_2$ |
| Conjunction (inner) | `, and` / `, but` / `, whereas` | $S_1 \land S_2$ |

**Examples:**
- "A user logs-in." (Simple)
- "A user logs-in, and the system verifies credentials." (Conjunction)
- "A user logs-in, and the system verifies credentials, or the connection times-out." (DNF)

**Design Rationale:** Natural language at sentence level improves readability; the comma serves as a visual boundary closing each simple sentence.

---

## 3. Verb Phrase (VP) Coordination

Within a simple sentence, VPs MUST be coordinated using symbolic operators. Plain English conjunctions are forbidden except for the fixed forms `either...or` and `neither...nor`.

### 3.1 VP Operators

| Symbol | Meaning | Logic |
|:---:|---|---|
| `&&` | Parallel conjunction | $VP_1 \land VP_2$ (simultaneous/unordered) |
| `&& then` | Sequential conjunction | $VP_1 \land VP_2 \land (t(VP_1) < t(VP_2))$ |
| `&&/` | Inclusive disjunction | $VP_1 \lor VP_2$ |

**Fixed English forms (VP coordination only):**

| Form | Meaning | Logic |
|---|---|---|
| `either` VP₁ `, or` VP₂ | Exclusive disjunction | $VP_1 \oplus VP_2$ |
| `neither` VP₁ `, nor` VP₂ | Negative conjunction | $\lnot VP_1 \land \lnot VP_2$ |

**Note:** In VP coordination, `or` and `nor` MUST be preceded by a comma. These forms are unambiguous: at sentence level, `either`/`neither` begins the sentence; at VP level, the subject precedes them.

### 3.2 Precedence and Associativity

All VP operators have **equal precedence** and are **left-associative**.

| Expression | Parses as |
|------------|-----------|
| `A && B &&/ C` | `((A && B) &&/ C)` |
| `A &&/ «B && C»` | `(A &&/ (B && C))` — guillemets override |

### 3.3 Prepositional Phrase Attachment

| Rule | Description | Example |
|------|-------------|---------|
| `of`-clauses → preceding NP | Always binds to noun | "The size of the file exceeds the limit." |
| Other PPs → preceding verb | Default binding | "A user sends a message to the server." |
| No preceding verb → NP | Context-dependent | "a message to the server" (NP-only context) |
| Guillemets override | Explicit attachment | "A user sends «a message to the server»." |
| Object-position restriction | NP in object position MUST NOT contain non-`of` PP unless guillemeted | Ensures deterministic parsing |

### 3.4 Adverb Placement and Scope

| Position | Description |
|----------|-------------|
| Pre-verbal | Immediately before the verb |
| Post-verbal | At end of VP (after object and all PPs) |

**Rules:**
- At most one adverb phrase per VP or guillemeted VP group
- Adverb in simple VP → modifies only that VP
- Adverb immediately before/after `«...»` → modifies entire group
- Adverbs may use coordination with `&`, `/`, `&/`

### 3.5 Modal Auxiliaries

| Modal | Meaning | Example |
|-------|---------|---------|
| `must` | Obligation | "The system must validate the input." |
| `must not` | Prohibition | "The system must not store plaintext passwords." |
| `may` | Permission | "A user may export data." |
| `may preferably` / `preferably may` | Permission but expected | "A user may preferably use two-factor authentication." |
| `should` / `should not` | Recommendation | "The system should log all events." |
| `is/are expected to` | Expectation | "The system is expected to respond within 5 seconds." |
| `is/are able to` / `is/are not able to` | Capability | "The operator is able to restart the server." |

**Rules:**
- Modals attach to closest verb (narrow scope by default)
- Use guillemets to scope over coordinated VPs
- `may not` is undefined—use `must not` for prohibition
- Agreement: singular subjects use `is`; plural use `are`

### 3.6 VP Negation

| Form | Usage |
|------|-------|
| NP `does not` VP | Singular subjects |
| NP `do not` VP | Plural subjects |

**Rules:**
- Negation attaches to closest verb (narrow scope)
- Use guillemets for wider scope
- VP negation MUST NOT combine with modal auxiliaries

**Contractions:** Sloj permits contracted forms as alternatives to the full forms. See Section 3.6.1.

#### 3.6.1 Contractions

Sloj allows contracted forms for common negations and auxiliaries to improve readability while maintaining unambiguous parsing.

| Full Form | Contracted Form | Context |
|-----------|----------------|---------|
| `is not` | `isn't` | Copula negation, passive negation |
| `are not` | `aren't` | Copula negation, passive negation |
| `does not` | `doesn't` | VP negation (singular) |
| `do not` | `don't` | VP negation (plural) |
| `has not` | `hasn't` | Present perfect negation (singular) |
| `have not` | `haven't` | Present perfect negation (plural) |
| `will not` | `won't` | Future negation |
| `must not` | `mustn't` | Prohibition modal |
| `should not` | `shouldn't` | Recommendation modal |
| `cannot` | `can't` | Capability negation (alternative to `is not able to`) |

**Rules:**
- Contracted and full forms are semantically equivalent
- Both forms may be used interchangeably within a document
- Contractions follow the same scope and attachment rules as their full forms
- `can't` is an alternative to `is not able to` / `are not able to` (singular/plural agreement applies)

**Examples:**
- "The system doesn't validate the input." (= "The system does not validate the input.")
- "The files aren't deleted." (= "The files are not deleted.")
- "The system mustn't store passwords." (= "The system must not store passwords.")
- "The user can't export data." (= "The user is not able to export data.")

### 3.7 VP List Coordination (Markdown)

A subject-scoped VP list is governed by a structural `:` at end of line, followed by a Markdown list.

| Form | Syntax | Meaning |
|------|--------|---------|
| Parallel conjunction (default) | `NP:` + unmarked list | All VPs, unordered |
| Sequential conjunction | First unmarked, then `- then VP` | Ordered sequence |
| Exclusive disjunction | `NP either:` + `- or VP` items | Exactly one |
| Inclusive disjunction | `NP:` + `- or VP` items (no `either`) | At least one |
| Negative conjunction | `NP neither:` + `- nor VP` items | None of the VPs |

**Rules:** First item is unmarked; subsequent items carry markers (`then`/`or`/`nor`). VP lists MUST be final element; may follow inline VP coordination with `&&:`, `&& then:`, or `&&/:`.

### 3.8 Present Perfect

Permitted **only** in conditional antecedents and relative clauses.

| Form | Example |
|------|---------|
| NP `has`/`have` VP | "...if a session has expired..." |
| NP `has`/`have been` Comp | "...if the card has been valid..." |
| NP `has`/`have not` VP | "...if the system has not responded..." |
| NP `has`/`have never` VP | "...if the account has never logged-in..." |

**Semantics:** Asserts existence of prior time-point where predicate held; does NOT assert state persistence. Use present tense for current state.

### 3.9 Unified Verb Syntax

All verbs follow a unified pattern—no grammar-level copula distinction.

| Pattern | Examples |
|---------|----------|
| NP Verb | "A dog runs." |
| NP Verb Arg | "John runs a company." / "The light becomes green." / "The card is validated." |
| NP Verb Arg PP* | "The file is stored in the cache." |

**Arg types:** NP, Adj, PastPart, PP, String. Semantic validity is checked by the lexicon/semantic layer, not grammar.

### 3.10 Passive Voice

Passive is a semantic interpretation of `is`/`are`/`be` + PastParticiple.

| Form | Example |
|------|---------|
| Basic passive | "The card is validated." |
| Passive negation | "The card is not validated." |
| Passive with agent | "The card is validated by the system." |
| Infinitive passive | "The card must be validated." |
| Coordinated | "The card is «validated & logged»." |

**Use passive when:** agent is unknown/irrelevant, focus should be on patient, or agent is implementation-dependent.

---

## 4. Temporal and Future Statements

### 4.1 Basic Future Forms

| Form | Meaning | Example |
|------|---------|---------|
| NP `will` VP | Simple future | "The system will respond." |
| NP `will always` VP | Universal future | "The system will always be available." |
| NP `will eventually` VP | Eventual future | "Every request will eventually be processed." |
| NP `will immediately` VP | Next time-point | "The counter will immediately be incremented." |
| NP `will then` VP | Sequential (as soon as possible) | "The system will then validate the input." |
| NP `will never` VP | Negative universal | "The system will never be in an error-state." |
| NP `will not` VP | Simple negative | "The system will not respond." |

**Note:** Time is discrete; "immediately" means "at the next time-point."

### 4.2 Temporal Scopes

| Form | Meaning |
|------|---------|
| `before` Condition `,` NP `will` VP | Before condition holds |
| `after` Condition `,` NP `will` VP | After condition holds |
| `after` C₁ `and before` C₂ `,` NP `will` VP | Between two conditions |

**Rules:** Condition uses present tense or present perfect; future is forbidden in conditions. Comma after condition is required.

### 4.3 Continuous Temporal Conditions

| Form | Meaning |
|------|---------|
| NP `will` VP `until` Condition | Continue until |
| NP `will` VP `unless` Condition | Continue unless |
| `while` Condition `,` NP `will` VP | Continue while |

`while` statements support Markdown lists (parallel, sequential, exclusive, inclusive) following the same rules as VP lists.

### 4.4 Temporal Sequences

| Form | Example |
|------|---------|
| `first` S `, then` S | "First a customer enters a card, then the system verifies it." |
| `first` S `, then` S `, finally` S | "First the card is inserted, then the PIN is entered, finally the transaction is processed." |

### 4.5 Temporal Bounds (Durations)

| Form | Meaning |
|------|---------|
| NP `will` VP `within` Duration | Complete within time |
| NP `will` VP `for` Duration | Continue for duration |
| NP `will` VP `every` Duration | Periodic repetition |
| NP `will` VP `at least every` Duration | Maximum period |
| NP `will` VP `at most every` Duration | Minimum period |
| NP `will` VP `every` D₁ `to` D₂ | Period range |

**Duration units:** `nanosecond(s)`/`ns`, `millisecond(s)`/`ms`, `second(s)`/`sec`, `minute(s)`/`min`, `hour(s)`/`hr`, `day(s)`, `week(s)`, `month(s)`, `quarter(s)`, `year(s)`/`yr`

### 4.6 Scope and Coordination

Future auxiliaries attach to closest verb. Use guillemets to scope over coordinated VPs. All verbs in group use base (infinitive) form.

---

## 5. Existential Sentences

| Form | Example |
|------|---------|
| `There is` NP | "There is a dog." |
| `There are` NP | "There are three white cats." |
| `There is not` NP | "There is not a valid token in the request." |
| `There are not` NP | "There are not enough resources." |

**Rules:** Agreement by number; NP is typically indefinite; optional locative PP may follow.

---

## 6. Comparisons

Comparisons express relationships between entities or values using comparative and superlative forms.

### 6.1 Comparative Constructions (Adjectives)

| Form | Example |
|------|---------|
| NP `is` ComparativeAdj `than` NP | "Tank A is larger than Tank B." |
| NP `is` ComparativeAdj `than` Number | "The temperature is greater than 100." |

**Note:** Comparative adjectives (e.g., `larger`, `greater`, `faster`, `smaller`) are defined in the lexicon.

### 6.2 Comparative Constructions (Adverbs)

| Form | Example |
|------|---------|
| NP VP ComparativeAdv `than` NP | "Process A completes more-quickly than Process B." |
| NP VP ComparativeAdv `than` NP VP | "The server responds faster than the client requests." |

**Note:** Comparative adverbs (e.g., `faster`, `more-quickly`) are defined in the lexicon.

### 6.3 Superlative Constructions (Adjectives)

| Form | Example |
|------|---------|
| NP `is the` SuperlativeAdj | "Tank A is the largest." |
| NP `is the` SuperlativeAdj `of` NP | "Tank A is the largest of the three tanks." |

### 6.4 Superlative Constructions (Adverbs)

| Form | Example |
|------|---------|
| NP VP `the` SuperlativeAdv | "Process A completes the most-quickly." |
| NP VP `the` SuperlativeAdv `of` NP | "Process A completes the fastest of all processes." |

### 6.5 Equality and Inequality

| Form | Example |
|------|---------|
| NP `is equal to` Number | "The priority is equal to 3." |
| NP `is equal to` NP | "The status is equal to the previous-status." |
| NP `is not equal to` Number | "The count is not equal to 0." |
| NP `is not equal to` NP | "The result is not equal to the expected-value." |
| NP `is not equal to` String | "The status is not equal to \"pending\"." |

### 6.6 Range Comparisons

| Form | Example |
|------|---------|
| NP `is between` NP | "The level is between the limits." |
| NP `is between` NP `&` NP | "The level is between the min-limit & the max-limit." |

**Notes:** 
- `between` is followed by a noun phrase (NP), which may be coordinated with `&`
- Bare numbers are not allowed; use measurement phrases: "between 30 seconds & 40 seconds" or "between «30 & 40» degrees"
- Inclusive bounds: [min, max]
- Full support requires measurement phrases (Task 2.7)

### 6.7 Disambiguation Notes

**`less` and `more`:**
- Followed by `than` → comparative: "The count is less than 5."
- Followed by mass noun → determiner: "Less water is available."

**`than` keyword:**
- In comparisons, `than` is a syntactic marker introducing the comparand, not a preposition.

**`of` in superlatives:**
- Introduces the comparison domain: "the largest of the three tanks."

---

## 7. Conditional Statements

| Keyword | Semantics | Logic |
|---------|-----------|-------|
| `if` | Material conditional | C → S |
| `whenever` | Universal reactive | ∀t (C(t) → S(t)) |
| `once` | One-time trigger with persistent effect | ∃t (C(t) ∧ ∀t' ≥ t : S(t')) |
| `each time` | Repeated trigger | Per occurrence |

**Shared rules:**
- Condition: present tense or present perfect; future forbidden
- Consequence: present tense or future (`will` + VP); present perfect forbidden
- Comma before `then` is required

### 7.1 Otherwise Clause

| Form | Meaning |
|------|---------|
| `Otherwise,` Statement | Holds when preceding condition is false |

**Rules:** Must immediately follow conditional; binds to most recent condition.

### 7.2 Exception Forms

| Form | Equivalent |
|------|------------|
| Statement `, except when` Condition | `if not` Condition `, then` Statement |
| Statement `, except if` Condition | Same as above |

### 7.3 Conditional Statement Lists (Markdown)

Conditionals support Markdown lists for multiple consequences, following the same coordination rules as VP lists (parallel, sequential, exclusive, inclusive).

---

## 8. Noun Phrase (NP) Coordination

### 8.1 NP Operators

| Symbol | Meaning | Logic |
|:---:|---|---|
| `&` | Conjunction | $NP_1 \land NP_2$ (both) |
| `/` | Exclusive disjunction | $NP_1 \oplus NP_2$ (one, not both) |
| `&/` | Inclusive disjunction | $NP_1 \lor NP_2$ (one, both, or either) |

**Note:** Exclusive disjunction `/` is the default for NPs (unlike ambiguous English "or").

### 8.2 Precedence

All NP/Adj/Adv operators have **equal precedence** and are **left-associative**. Use guillemets only to override default evaluation or attach modifiers to groups.

### 8.3 Noun Types

| Type | Determiner | Example |
|------|------------|---------|
| Common noun | Required | "A user runs." |
| Proper noun | Forbidden | "John runs." |
| Unique noun | Required | "The GPS starts." |
| Mass noun | Mass determiner required (`the`, `some`, `no`, `more`, `less`, `enough`) | "Some data leaks." |

**Proper noun pattern:** Capital letter followed by letters, digits, or hyphens (e.g., `Alice`, `Server1`, `Disk-1`).

**Gender:** Male/Female (recognized names), Neutral (all others including identifiers like `X1`).

**Nominalizations:** Event nominals (e.g., `validation`, `reading`, `transmission`) and property nominals (e.g., `coldness`, `validity`, `size`) are treated as regular common nouns. They use standard NP syntax with `of`-clauses for arguments: "the validation of the input", "the coldness of the water". The lexicon categorizes these for semantic processing.

### 8.4 Special Determiners

| Determiner | Usage |
|------------|-------|
| `a distinct` | Must be in scope of universal quantifier; entity distinct per binding |
| `enough` | For plural common nouns or mass nouns |

### 8.5 Pronouns

|  | Subjective | Objective | Possessive | Reflexive Possessive |
|--|------------|-----------|------------|---------------------|
| Male | `he` | `him` | `his` | `his own` |
| Female | `she` | `her` | `hers` | `her own` |
| Neutral | `it` | `it` | `its` | `its own` |
| Plural | `they` | `them` | `theirs` | `their own` |

**Resolution:** Refers to closest preceding antecedent of matching gender. Subject position requires subjective pronoun.

**Reflexive actions:** Use `self-` prefix on verb instead of reflexive pronouns (e.g., "Bob self-authenticates." not "Bob authenticates himself.").

### 8.6 Binding

| Form | Example |
|------|---------|
| Implicit (juxtaposition) | "A user Alice logs-in." |
| Explicit (appositive) | "A user, Alice, logs-in." |
| Explicit (named/called) | "A user, named Alice, logs-in." |

Binds proper noun to NP; subsequent references via proper noun or matching pronouns.

### 8.7 Measurement Phrases

Measurement phrases make mass nouns countable by specifying a quantity with a unit.

| Form | Example |
|------|---------|
| Number Unit `of` MassNoun | "3 liters of water" |

**Agreement:**
- Singular (Number = 1): "1 liter of water **is** warm. **It** is pure."
- Plural (Number ≠ 1): "3 liters of water **are** warm. **They** are pure."

**Units:**
- Units are lexicon entries (e.g., `liter`, `megabyte`, `kilogram`, `second`, `meter`)
- Units follow standard noun morphology (singular/plural)

**Usage:**
- As subject: "500 megabytes of data are cached."
- In comparisons: "The temperature is between 0 degrees & 100 degrees."
- With `of`: "The amount of water is 5 liters."

### 8.8 Mass Comparisons

Mass comparisons express relative quantities of mass nouns.

| Form | Example |
|------|---------|
| `more` MassNP `than` MassNP | "The tank contains more water than fuel." |
| `less` MassNP `than` MassNP | "The process uses less memory than bandwidth." |
| `the amount of` MassNP | "The amount of data is large." |

**Note:** `the amount of` converts a mass noun to a countable quantity that can be compared.

### 8.9 Partitive Constructions

**Proportion partitives (mass or plural count):**

| Partitive | Example |
|-----------|---------|
| `some of the` | "Some of the water evaporates." |
| `all of the` | "All of the data is backed up." |
| `none of the` | "None of the fuel is consumed." |
| `most of the` | "Most of the memory is allocated." |
| `half of the` | "Half of the storage is reserved." |
| `N% of the` | "30% of the data is corrupted." |

**Count partitives:**

| Form | Example |
|------|---------|
| `one of the` / `only one of the` PluralNP | "The operator may push one of the buttons." |
| `N of the` / `exactly N of the` / `only N of the` PluralNP | "2 of the buttons are pressed." |
| `at least N of the` / `at most N of the` PluralNP | "At least 2 of the buttons are pressed." |
| `between N and M of the` PluralNP | "Between 3 and 5 of the LEDs blink." |

### 8.10 Exception Suffix

| Form | Meaning |
|------|---------|
| NP₁ `except` NP₂ | NP₂ excluded from NP₁ |
| NP₁ `except the one that` VP | Pro-form (singular) |
| NP₁ `except the ones that` VP | Pro-form (plural) |

**Banned pro-forms:** `one`/`ones` as head nouns are forbidden except in exception constructions.

### 8.11 Lists/Enumeration

Lists provide a readable way to specify multiple values or items. Three keywords determine the coordination semantics.

#### 8.11.1 List Keywords

| Keyword | Semantics | Logic |
|---------|-----------|-------|
| `one of:` | Exclusive disjunction | Exactly one: $A \oplus B \oplus C$ |
| `any of:` | Inclusive disjunction | At least one: $A \lor B \lor C$ |
| `all of:` | Conjunction | All: $A \land B \land C$ |
| `none of:` | Negated disjunction | None: $\lnot A \land \lnot B \land \lnot C$ |

#### 8.11.2 Inline Lists

Inline lists use commas to separate items. The list ends at the sentence period.

| Form | Example |
|------|---------|
| Simple | `The value is one of: 3, "pippo", xld.` |
| With NPs | `The actor is one of: a user, an admin, a guest.` |
| Guillemeted | `The value is one of: «3, "pippo"», or one of: «7, 8».` |

**Guillemets** are required when:
- Multiple lists appear in one sentence
- The list is followed by more content

#### 8.11.3 Markdown Lists

Markdown lists use `-` prefix for each item. Each item ends with `.`.

```
The value is one of:
- 3.
- "pippo".
- xld.
```

Markdown lists MUST be the final element of the sentence (same as VP lists).

#### 8.11.4 Open vs Exhaustive Lists

By default, lists are **exhaustive** (closed-world): the items listed are ALL possible values. To indicate an **open** list (where other values may exist), append an ellipsis as the final element.

| Form | Semantics | Example |
|------|-----------|---------|
| No ellipsis | Exhaustive (closed) | `one of: A, B, C.` — only A, B, or C |
| With `...` | Open (extensible) | `one of: A, B, C, ...` — A, B, C, or others |
| With `…` | Open (extensible) | `one of: A, B, C, …` — same as above |

**Inline form:**
```
The state is one of: idle, running, stopped.        (exhaustive)
The state is one of: idle, running, stopped, ...    (open)
```

**Markdown form:**
```
The state is one of:       (exhaustive)
- idle.
- running.
- stopped.

The state is one of:       (open)
- idle.
- running.
- ...
```

**Notes:**
- Both `...` (three periods) and `…` (U+2026 ellipsis) are accepted
- The ellipsis must be the **final element** of the list
- In Markdown lists, the ellipsis item may optionally end with a period: `- ....` or `- ...`

#### 8.11.5 List Rules

| Rule | Description |
|------|-------------|
| **Items** | Full NPs, numbers, string literals, or proper nouns |
| **Minimum** | At least one item required (ellipsis alone is invalid) |
| **No nesting** | Lists cannot contain lists |
| **No empty** | Empty lists are forbidden |
| **Ellipsis** | Optional `...` or `…` as final element indicates open list |

---

## 9. String Literals

### 9.1 Forms and Escaping

| Delimiter | Example | Escape |
|-----------|---------|--------|
| Double quotes | `"active"` | `\"`, `\\` |
| Backticks | `` `f(x)` `` | `` \` ``, `\\` |

### 9.2 Pronoun Invisibility

String literals are **invisible** to pronoun resolution. Pronouns skip strings and refer to nearest NP.

### 9.3 Usage Contexts

Comparisons, file names/paths, technical notation, literal values, inside lists.

**Coordination:** Strings do NOT participate in NP coordination.

### 9.4 Fenced Code Blocks

Markdown code blocks (` ``` ` or `~~~`) appear at **sentence level** only (before/after sentences, not inline).

**Rules:**
- Invisible to pronoun resolution
- Do not participate in VP/NP coordination, relative clauses, or conditionals
- Purely documentary elements

---

## 10. Purpose Clauses

Purpose clauses express the **intent, rationale, or purpose** behind a requirement. They are introduced by the fixed keyword phrase `in order to`.

### 10.1 Forms

| Form | Structure | Example |
|------|-----------|---------|
| Prefix | `In order to` PurposeVP `,` S `.` | "In order to comply with regulations, the lever must be locked." |
| Suffix | S `, in order to` PurposeVP `.` | "The system must log all operations, in order to ensure auditability." |
| Incidental | S₁ `, in order to` PurposeVP `,` S₂ `.` | "The system must log operations, in order to ensure auditability, and it must store logs for 90 days." |

### 10.2 Rules

| Rule | Description |
|------|-------------|
| **Non-normative** | Purpose clauses do NOT change the formal logic mapping of the sentence |
| **Infinitive VP** | PurposeVP must be an infinitive VP (base verb form) with no explicit subject |
| **No commas** | The purpose clause must NOT contain a comma token |
| **Comma delimited** | Comma delimiting is mandatory |
| **Documentation only** | Purpose clauses express intent/rationale, NOT requirements |

### 10.3 Semantics

Purpose clauses are **documentary elements**:
- They do NOT participate in the formal logic mapping
- They do NOT create additional requirements
- Tools MAY retain them for analysis, traceability, or documentation
- They explain **why** a requirement exists, not **what** the requirement is

**Example:**
```
The system must encrypt all data, in order to protect user privacy.
```
- **Requirement**: "The system must encrypt all data"
- **Purpose**: "to protect user privacy" (documentary)

---

## 11. Annotations

Annotations provide **metadata** for requirements without affecting their formal semantics. They use the `[[ ... ]]` syntax and appear on their own line(s) immediately before the sentence or code block they annotate.

### 11.1 Syntax

| Form | Example |
|------|---------|
| Key-value pairs | `[[ id: REQ-001, priority: high ]]` |
| Free text | `[[ This requirement comes from the customer SLA. ]]` |
| Multiple lines | `[[ id: REQ-001 ]]`<br>`[[ priority: high ]]` |

### 11.2 Rules

| Rule | Description |
|------|-------------|
| **Placement** | Annotations appear on their own line(s) immediately before the element they annotate |
| **Non-normative** | Annotations do NOT affect the formal logic mapping or semantics |
| **Content** | May contain key-value pairs (`key: value`) separated by commas, or free text |
| **No nesting** | Annotations cannot be nested |
| **Whitespace** | Leading/trailing whitespace in values is trimmed |

### 11.3 Common Use Cases

**Requirements Management:**
```
[[ id: REQ-001, priority: high, author: alice ]]
The system must respond within 100 milliseconds.
```

**Traceability:**
```
[[ links: JIRA-123, TEST-456 ]]
The user can submit & save the form.
```

**Rationale/Context:**
```
[[ This requirement addresses the performance issue reported in Q3. ]]
The cache must be cleared every 5 minutes.
```

**Versioning:**
```
[[ version: 2.1, date: 2024-01-15, status: approved ]]
The API must support OAuth 2.0 authentication.
```

### 11.4 Semantics

Annotations are **documentary elements**:
- They do NOT participate in the formal logic mapping
- They do NOT create or modify requirements
- Tools MAY extract, validate, or use them for requirements management
- They provide metadata for tooling, traceability, and documentation

**Example:**
```
[[ id: REQ-042, priority: critical ]]
The system must encrypt all data.
```
- **Requirement**: "The system must encrypt all data"
- **Metadata**: `id: REQ-042, priority: critical` (documentary)

### 11.5 Multiple Annotations

Multiple annotations can be applied to a single element:
```
[[ id: REQ-001 ]]
[[ priority: high ]]
[[ author: alice, date: 2024-01-15 ]]
The system must validate all inputs.
```

All annotations immediately preceding an element are associated with that element.

---

## 12. Explicit Grouping (Guillemets)

Use `«...»` to explicitly delimit phrases when overriding left-to-right evaluation or attaching modifiers to coordinated groups.

**Nesting:** Allowed when required, but prefer minimizing depth. Deep nesting suggests restructuring into multiple sentences.

**Examples:**
- `««a cat & a dog» / a mouse»` vs `«a cat & «a dog / a mouse»»`
- `«the old men & the old women»` (both old) vs `«the old men & the women»` (only men old)

---

## 13. Document Structure and Markdown Integration

Sloj requirements are typically embedded in Markdown documents. This section provides an overview of how Sloj integrates with Markdown.

### 13.1 Integration Points

Markdown constructs that are part of Sloj grammar:
- **Markdown lists**: Used for VP lists (Section 3.7), NP lists (Section 8.11), conditional lists (Section 7.3), and while lists (Section 4.3)
- **Fenced code blocks**: Documentary elements (Section 9.4)
- **Annotations**: Metadata blocks (Section 11)

### 13.2 Pure Markdown Formatting

The following Markdown elements are formatting only and not part of Sloj syntax:
- **Headings** (`#`, `##`, etc.) - Document organization
- **Tables** (all content) - Metadata and documentation
- **Emphasis** (`*italic*`, `**bold**`) - Not allowed in Sloj text
- **Links, images** - Not allowed in Sloj text (use string literals for URLs)
- **Blockquotes, horizontal rules** - Documentary elements

These elements provide document structure but are ignored by Sloj parsers.

### 13.3 Sloj Text vs Free Text

**Sloj text** (parsed and validated):
- Paragraph text forming complete Sloj sentences
- List items when part of Sloj list syntax (introduced by `:`)
- Annotation content

**Free text** (not parsed):
- Headings, table content, blockquotes
- Links, images, inline code (in free text contexts)
- Documentary paragraphs

### 13.4 Detailed Specification

For complete rules on Sloj-in-Markdown integration, including:
- Parsing models and extraction algorithms
- Ambiguity resolution (`:`, `-`, `.`, backticks)
- Indentation and whitespace rules
- Validation and linting requirements
- Tool implementation guidelines

See the separate specification: [`sloj-spec-markdown.md`](sloj-spec-markdown.md).

---

## Appendix A: Grammar (EBNF)

Refer to the file `sloj-spec-grammar.bnf`
