# SngL Language Specification

**Version:** 0.1-draft  
**Status:** Working Draft  
**Date:** 2026-01-24  

---

## About This Document

This document is authoritative for the SngL syntax rules and determinism rules.

Related specifications:

- Keywords are defined in `sngl-spec-keywords.md`.
- SngL-in-Markdown rules are defined in `sngl-spec-markdown.md`.
- Lexicon storage format and lexicon interpretation rules are defined in `lexicon/docs/lexicon-spec.md`.

Normative keywords:
`MUST`, `MUST NOT`, `SHOULD`, `SHOULD NOT`, `MAY`.

## At a Glance

- Use determiners for all common nouns.
- Use `that` for relative clauses. Do not use `who` or `which`.
- Use present tense for main clauses. Use future tense for future behavior.
- Use present perfect only in conditional antecedents and relative clauses.
- Use SngL coordination operators: `&`, `/`, `&/`, `&&`, `&& then`.
- Use `<<...>>` or `«... »` to make scope and attachment explicit.
- Use a subject-scoped VP list for 3 or more VPs with the same subject.
- Treat unknown content words as an error.

## Table of Contents

- [SngL Language Specification](#sngl-language-specification)
  - [About This Document](#about-this-document)
  - [At a Glance](#at-a-glance)
  - [Table of Contents](#table-of-contents)
  - [0. Introduction](#0-introduction)
    - [0.1 Time Model](#01-time-model)
  - [1. Lexical elements](#1-lexical-elements)
    - [1.1 Determiners](#11-determiners)
    - [1.2 Lexicon contract](#12-lexicon-contract)
      - [1.2.1 Keywords vs content words](#121-keywords-vs-content-words)
      - [1.2.2 Required lexicon entry types and features](#122-required-lexicon-entry-types-and-features)
      - [1.2.3 Case-sensitivity and keyword collisions](#123-case-sensitivity-and-keyword-collisions)
      - [1.2.4 Hyphenation and multi-word content words](#124-hyphenation-and-multi-word-content-words)
      - [1.2.5 Capitalization and name normalization](#125-capitalization-and-name-normalization)
    - [1.3 Common Nouns (classes of entities)](#13-common-nouns-classes-of-entities)
    - [1.4 Proper Nouns](#14-proper-nouns)
    - [1.5 Unique Nouns](#15-unique-nouns)
    - [1.6 Pronouns (reference to known entities)](#16-pronouns-reference-to-known-entities)
      - [1.6.1 Pronouns reference rules](#161-pronouns-reference-rules)
      - [1.6.2 Pronunciation of the common singular pronuns:](#162-pronunciation-of-the-common-singular-pronuns)
    - [1.7 Nominalizations](#17-nominalizations)
    - [1.8 Named References](#18-named-references)
    - [1.9 Numbers](#19-numbers)
    - [1.10 Verbs](#110-verbs)
    - [1.11 Adjectives (noun modifiers)](#111-adjectives-noun-modifiers)
    - [1.12 Adverbs](#112-adverbs)
    - [1.13 Relative Clauses](#113-relative-clauses)
    - [1.14 Prepositional Phrases](#114-prepositional-phrases)
    - [1.15 Of-Clause](#115-of-clause)
    - [1.16 String Literals](#116-string-literals)
    - [1.17 Lists](#117-lists)
      - [Closed-World list (Exhaustive)](#closed-world-list-exhaustive)
      - [Open-World list (Extensible)](#open-world-list-extensible)
    - [1.18 Annotations](#118-annotations)
    - [1.19 Mass Nouns (Uncountable Substances)](#119-mass-nouns-uncountable-substances)
      - [Grammatical Behavior](#grammatical-behavior)
      - [Allowed Determiners](#allowed-determiners)
      - [Dual Mass/Count Nouns](#dual-masscount-nouns)
    - [1.19.1 Quantified Mass (Measurement Phrases)](#1191-quantified-mass-measurement-phrases)
      - [Units](#units)
    - [1.19.2 Partitive Constructions](#1192-partitive-constructions)
    - [1.19.2.1 Proportion Partitives (Mass or Plural Count)](#11921-proportion-partitives-mass-or-plural-count)
    - [1.19.2.2 Count Partitive Constructions](#11922-count-partitive-constructions)
    - [1.19.3 Mass Comparisons](#1193-mass-comparisons)
  - [2. Present Perfect (Prior Events)](#2-present-perfect-prior-events)
    - [2.1 Positive Forms](#21-positive-forms)
    - [2.2 Negation forms](#22-negation-forms)
    - [2.3 Examples](#23-examples)
  - [3. Predicates (Truth values)](#3-predicates-truth-values)
    - [3.1 Properties/actions](#31-propertiesactions)
    - [3.2 Negation](#32-negation)
    - [3.3 Sentence Negation](#33-sentence-negation)
    - [3.4 Comparisons](#34-comparisons)
      - [3.4.1 Comparative constructions (adjectives):](#341-comparative-constructions-adjectives)
      - [3.4.2 Comparative constructions (adverbs):](#342-comparative-constructions-adverbs)
      - [3.4.3 Superlative constructions (adjectives):](#343-superlative-constructions-adjectives)
    - [Purpose Clauses (Rationale/Purpose)](#purpose-clauses-rationalepurpose)
  - [4. Conditional Statements](#4-conditional-statements)
    - [Tense and Aspect in Conditionals](#tense-and-aspect-in-conditionals)
  - [5. Deontic Statements (Obligations, Permissions, Prohibitions, Capabilities)](#5-deontic-statements-obligations-permissions-prohibitions-capabilities)
  - [6. Temporal Statements (Future Tense)](#6-temporal-statements-future-tense)
    - [6.1 Basic Temporal](#61-basic-temporal)
    - [6.2 Temporal with Scopes](#62-temporal-with-scopes)
    - [6.3 Conditional Time Continuous](#63-conditional-time-continuous)
    - [6.4 Temporal Sequences](#64-temporal-sequences)
    - [6.5 Temporal with Bounds](#65-temporal-with-bounds)
  - [7. Coordination](#7-coordination)
    - [7.1 Noun phrases, adjectives, and adverbs Coordination](#71-noun-phrases-adjectives-and-adverbs-coordination)
      - [7.1.1 Conjunction](#711-conjunction)
      - [7.1.2 Exclusive Disjunction](#712-exclusive-disjunction)
      - [7.1.3 inclusive Disjunction](#713-inclusive-disjunction)
      - [7.1.4 Quantification over coordination](#714-quantification-over-coordination)
      - [7.1.5 Negation over coordination](#715-negation-over-coordination)
      - [7.1.6 Multiple coordination](#716-multiple-coordination)
      - [7.1.7 Examples](#717-examples)
    - [7.2 Verb Phrase Coordination](#72-verb-phrase-coordination)
      - [7.2.1 Conjunction](#721-conjunction)
      - [7.2.2 Sequential Conjunction](#722-sequential-conjunction)
      - [7.2.3 Negative Conjunction](#723-negative-conjunction)
      - [7.2.4 Inclusive Disjunction](#724-inclusive-disjunction)
      - [7.2.5 Exclusive Disjunction](#725-exclusive-disjunction)
      - [7.2.6 Adverb Scope in VP Coordination](#726-adverb-scope-in-vp-coordination)
      - [7.2.7 Deontical, Temporal and Negation  Scope in VP Coordination](#727-deontical-temporal-and-negation--scope-in-vp-coordination)
      - [7.2.8 Multiple VP coordination.](#728-multiple-vp-coordination)
      - [7.2.9 List coordination](#729-list-coordination)
    - [7.3 Sentence-Level Coordination](#73-sentence-level-coordination)
      - [7.3.1 Conjunction](#731-conjunction)
      - [7.3.2 Negative conjunction](#732-negative-conjunction)
      - [7.3.3 Inclusive disjunction](#733-inclusive-disjunction)
      - [7.3.4 Exclusive disjunction](#734-exclusive-disjunction)
  - [8. Interpretation Rules](#8-interpretation-rules)
    - [8.1 Reference Resolution](#81-reference-resolution)
    - [8.2 Attachment Rules](#82-attachment-rules)
    - [8.3 Scope and Precedence](#83-scope-and-precedence)
    - [8.4 Agreement Rules](#84-agreement-rules)
    - [8.5 Temporal Interpretation](#85-temporal-interpretation)
  - [9. Ill-formed Constructions](#9-ill-formed-constructions)
  - [Appendix A: Glossary](#appendix-a-glossary)

## 0. Introduction

SngL is a controlled natural language designed for unambiguous software requirements specifications.
It retains readable English structure, but it is deterministically parseable (every sentence has exactly one parse tree) and formally grounded (every parse tree maps to exactly one logical formula).

This document specifies the syntax of SngL controlled natural language. 
The mapping to formal logic is specified in a different document. 

SngL text MUST be encoded in UTF-8. 

### 0.1 Time Model

SngL assumes a **discrete linear time** model:

- Time is a totally ordered sequence of discrete time-points: ..., t₋₂, t₋₁, t₀, t₁, t₂, ...
- Each time-point represents an atomic moment at which predicates hold or fail to hold.
- There is a distinguished **evaluation point** (the "now") relative to which tenses are interpreted.

| Tense           | Time-points | Interpretation                               |
|-----------------|-------------|----------------------------------------------|
| Present         | t₀          | Predicate holds at evaluation point          |
| Present perfect | t < t₀      | Predicate held at some prior time-point      |
| Future          | t > t₀      | Predicate will hold at some later time-point |

Durations (Section 6) map to contiguous spans of time-points. The keyword `immediately` denotes the next time-point (t₀ + 1).

Note: The present perfect asserts that an event occurred; it does not assert whether the resulting state persists. Use present tense to assert current state.

## 1. Lexical elements

This section defines the lexical units (tokens) of the language, including keywords, identifiers, and literals. It also specifies the lexicon contract that a tool uses for deterministic parsing and lint-checking.

### 1.1 Determiners

Determiners are mandatory; they assert the existence of entities or refer to previously mentioned entities.

| Keyword             | Semantic Role        | Description                         | Example                                               |
|---------------------|----------------------|-------------------------------------|-------------------------------------------------------|
| `the`,`this`        | Reference            | References known entity (singular)  | "The customer places an order."                       |
| `these`, `those`    | Reference            | References known entity (plural)    | "These requests are processed."                       |
| `a`, `an`           | Existential          | Introduces new entity               | "A user logs in."                                     |
| `some`              | Existential          | One or more entities                | "Some users authenticate."                            |
| `every`             | Existential          | All entities (wide temporal scope)  | "Every request must be validated."                    |
| `each`              | Existential          | All entities (narrow temporal scope)| "Each session expires after 30 minutes."              |
| `all`               | Existential          | Entire set                          | "All files are backed up."                            |
| `no`                | Existential          | Zero entities                       | "No unauthorized user can access the system."         |
| `a distinct`        | Existential          | One-to-one mapping                  | "Every customer has a distinct account-number."       |
| `at least` N        | Existential          | Minimum count                       | "At least 3 servers must be running."                 |
| `at most` N         | Existential          | Maximum count                       | "At most 5 login-attempts are allowed."               |
|  N (number)         | Existential          | Exact count                         | "Three backups are created daily."                    |
| `exactly` N         | Existential          | Exact count                         | "Exactly 2 administrators are required."              |
| `more than` N       | Existential          | Greater than count                  | "More than 10 users are online."                      |
| `less than` N       | Existential          | Less than count                     | "Less than 5 errors occurred."                        |
| `between` N `and` M | Existential          | Range count                         | "Between 3 and five retries are performed."           |
| `except`            | Negative Existential | Excludes part of specified entities | "Every user except administrators must authenticate." |
| `more`              | Mass Quantifier      | Greater quantity (mass nouns)       | "More memory is allocated."                           |
| `less`              | Mass Quantifier      | Lesser quantity (mass nouns)        | "Less bandwidth is available."                        |
| `enough`            | Mass Quantifier      | Sufficient quantity (mass nouns)    | "Enough storage is provisioned."                      |

Note: The determiners `a/an`, `every`, `each`, and cardinal numbers cannot be used directly with mass nouns. Use measurement phrases instead (see Section 1.19.1).

### 1.2 Lexicon contract

The lexicon defines SngL content-word word-forms and the grammatical features that a tool needs for deterministic parsing and deterministic lint-checking.

Any parser and any lint-checker MUST use the lexicon for:

1. Distinguishing content words from unknown words.
2. Agreement (for example: subject-verb agreement, determiner-noun agreement).
3. Reference resolution (pronoun-antecedent matching by number and gender).

#### 1.2.1 Keywords vs content words

The lexicon contains only content words.
SngL keywords (including determiners, pronouns, auxiliary verbs, and modal auxiliaries) are not defined in the lexicon.

The parser and the lint-checker MUST treat keyword matching as case-insensitive.
For example: `the` == `The` == `THE`.

#### 1.2.2 Required lexicon entry types and features

The lexicon MUST support exactly these eight entry types:

| Entry Type        | Required Features    |
|-------------------|----------------------|
| `N` (noun)        | Number, Gender       |
| `M` (mass_noun)   | Number, Gender       |
| `U` (unit)        | Number, Gender       |
| `Q` (unique_noun) | Number, Gender       |
| `P` (proper_noun) | Number, Gender       |
| `V` (verb)        | Tense, Transitivity  |
| `J` (adj)         | Comparison forms     |
| `D` (adv)         | Comparison forms     |

The lexicon MUST provide the following feature values:

| Feature      | Values                                                                              |
|--------------|-------------------------------------------------------------------------------------|
| Number       | `s` (singular),   `p` (plural),       `x` (invariant or unspecified)                |
| Gender       | `m` (masculine),  `f` (feminine),     `n` (neuter), `c` (common), `x` (unspecified) |
| Transitivity | `t` (transitive), `i` (intransitive), `c` (copular)                                 |
| Degree       | `p` (positive),   `c` (comparative) , `s` (superlative)                             |
| Tenses       | `b` (base),       `3` (present 3d person), `p` (past participle)                    |
| Comp. forms  | `p` (Positive),   `c` (Comparative),  `s` (Superlative)                             |  


Transitivity `c` (copula) is reserved for lexicon verbs that take a subject complement (NP or Adj), such as "seem" or "become". 
 
Note: The lexicon storage format and interpretation rules are specified in `lexicon/docs/lexicon-spec.md`.

get_lex(word [,type])

- get_lex("runs") -> "V3i"
- get_lex("rock") -> "Npn Vbt"
- get_lex("further") -> "Jc_"


#### 1.2.3 Case-sensitivity and keyword collisions

The parser and the lint-checker MUST treat lexicon word-form matching as case-sensitive.
For example: `MB` != `mb`.

A lexicon word-form MUST NOT match any single-token keyword (Appendix B.1) under case-insensitive comparison.
This rule applies in particular to `proper_noun` and `unique_noun` entries.

#### 1.2.4 Hyphenation and multi-word content words

All multi-word content words (compound nouns, phrasal verbs, multi-word adjectives, multi-word adverbs) MUST be hyphenated.

This rule does not apply to a proper name or a unique name that is written as a name sequence.
Section 1.2.5 defines the name-sequence normalization rule.

Particle movement in phrasal verbs is not supported.
For example, use `looks-up the word`, not `*looks the word up`.

#### 1.2.5 Capitalization and name normalization

A SngL document MAY contain a name sequence.
A name sequence is a maximal sequence of 2 or more consecutive word tokens where each token starts with an ASCII capital letter `A`-`Z` and none of the tokens is a keyword.

If the parser or the lint-checker reads a name sequence `W1 W2 ... Wn` then it MUST normalize it to the single token `W1-W2-...-Wn` before syntax parsing and before lexicon lookup.

After normalization, the normalized token MUST match a `proper_noun` entry or a `unique_noun` entry.
If it does not match such an entry then the parser MUST reject the input and the lint-checker MUST report an error.

Names MUST NOT rely on punctuation (for example `Corp.`).
Use a punctuation-free token (for example `Corp`) and use hyphenation for multi-part names.

### 1.3 Common Nouns (classes of entities)

All common nouns MUST be preceded by a determiner.
Compund words must be hyphenated.

Examples:
- "A customer places an order."
- "A card-holder inserts a card."

### 1.4 Proper Nouns

Names for individual entities.
All proper nouns are capitalized and not preceded by a determiner.

Examples:
- "John sends the message."
- "Acme Corp sends the message." (normalized to `Acme-Corp`)
- "The-Gateway forwards a request."

A proper name MAY be written as a name sequence in a document.
If it is written as a name sequence then the name sequence is normalized to a hyphenated token.

A name sequence MUST NOT contain keywords.
If a proper name contains a keyword token (for example `The`) then the proper name MUST be written as a single hyphenated token (for example `The-Gateway`).

Examples:
- "Frank Miller sends the message." (normalized to `Frank-Miller`)
- "Acme Corp sends the message." (normalized to `Acme-Corp`)

Examples: "Bob sends the message to Alice."

### 1.5 Unique Nouns 

Unique nouns are named entities that are unique in the context.
All Unique Nouns are capitalized and preceded by a determiner.

Examples: 
  - "The GPS must always be turned on."
  - "Each change-request must be approved by a Team-Leader."

A unique name MAY be written as a name sequence in a document.
If it is written as a name sequence then the name sequence is normalized to a hyphenated token (Section 1.2.5).

A name sequence MUST NOT contain keywords.
If a unique name contains a keyword token then the unique name MUST be written as a single hyphenated token.

Examples:
- "The Vice President approves." (normalized to `the Vice-President`)
- "The President Emeritus resigns." (normalized to `the President-Emeritus`)

Of-clauses create a scope for uniqueness:
 - "The CEO of Alfa and the CEO of Beta." (Two different CEO)
 - "The GPS on the car or the GPS on the phone." (Two different GPS)

### 1.6 Pronouns (reference to known entities)

- Only third person pronouns are allowed. 
- For gender-neutral (common) singular reference SngL uses a variation of Elverson pronouns prefixed with "y" : (yey, yem, yeir, yeirs, yemself).
- Singular "they" is not permitted.

|                         |                    Singular                    || Plural      |
| Case                    | Masculine | Feminine  | Neutral   | Common     || Common      |
|-------------------------|-----------|-----------|-----------|------------||-------------|
| Subjective              | `he`      | `she`     | `it`      | `yey`      || `they`      |
| Objective               | `him`     | `her`     | `it`      | `yem`      || `them`      |
| Possessive (determiner) | `his`     | `her`     | `its`     | `yeir`     || `their`     |
| Possessive (pronoun)    | `his`     | `hers`    | `its`     | `yeirs`    || `theirs`    |
| Reflexive               | `himself` | `herself` | `itself`  | `yemself`  || `themself`  |
| Possessive reflexive    | `his own` | `her own` | `its own` | `yeir own` || `their own` |

- The pronoun feature-set (number, gender, and case/role) is fixed by this list and is not stored in the lexicon.
- The word `own` in a possessive-reflexive determiner is a fixed marker in that construction.

- For parsing and linting, each possessive-reflexive determiner form MUST be treated as a fixed multi-token keyword phrase.
- There is no standalone token `own` reserved as a keyword.
  
#### 1.6.1 Pronouns reference rules

- Non-reflexive pronouns always refer to the closest, most specific (by gender and number) preceding noun EXCLUDING the subject of the current sentence. 
- Reflexive pronouns and reflexive possessives (itself, her own, yeir own, ...) always refer to the current sentence subject.

#### 1.6.2 Pronunciation of the common singular pronuns:

| Common     | IPA        |
|------------|------------|
| `yey`      | /jeɪ/      |
| `yem`      | /jem/      |
| `yeir`     | /jeɪr/     |
| `yeirs`    | /jeɪrz/    |
| `yemself`  | /jemˈsɛlf/ |

- The /j/ onset is the English "y" sound as in *yes*
- The diphthong /eɪ/ in *yey*, *yeir*, and *yeirs* matches *they*, *their*, *theirs*
- The /ɛ/ in *yem* and *yemself* matches *them*, *themself*
- Primary stress in *yemself* falls on the second syllable, mirroring *himself*, *herself*, *themself*

### 1.7 Nominalizations

Events, actions, and properties can be referred to as nouns.

Examples of nominalization nouns are: `insertion`, `failure`, `reading`, `validity`, `redness`.

If a word appears in the lexicon as both an adjective and a noun,
standard grammatical context determines the interpretation:

- After a determiner with no following noun → nominalization
- After a determiner before a noun → adjective

Examples:
 - "The public is informed." (`public` is a noun)
 - "The public notice is posted." (`public` is an adjective)

### 1.8 Named References

Proper nouns can be attached to known entities for later reference.
A special type of proper nouns, called `ID`, can be formed using a single capital letter and, possibly, adding a number: (A, X1, Z33).

 | Form                          | Example                                                                      |
 |-------------------------------|------------------------------------------------------------------------------|
 | NP `, named` Proper noun `,`  | "A user, named Alice, calls another user, named Bob. Bob responds to Alice." |
 | NP `, called` Proper noun `,` | "A user, called Alice, calls another user, called Bob. Bob responds to her." |
 | NP `, aka`  Proper noun `,`   | "A user, aka Alice, calls another user, aka Bob. He responds to Alice."      |
 | NP ID                         | "A user A calls another user B . B responds to A.", "The wire W1 connects the terminal T" |

The comma before the keywords is required. After the `ProperName`, a comma is required unless the sentence ends immediately after the name (in which case the final period closes the appositive). 

### 1.9 Numbers

Numbers can be expressed in many forms.

| Form       | Example                                        |
|------------|------------------------------------------------|
| Integer    | "43", "-2", "0"                                |
| Decimal    | "3.14", "-2.4", "0.86"                         |
| Scientific | "-1.4E12", "0.42E-2.5"                         |
| Literal    | "one", "tweleve", "fifty" (up to "ninetynine") |
| Expression | "{3+4}", "\arctan{3+\frac{y}{3}}"              |

Numbers are determiners.
Example:
 - "Five cats cross the road."

### 1.10 Verbs

Common verbs are content words defined in the lexicon.
Examples of verbs are: `own`, `insert`, `look-up`, `shut-down`.

Auxiliary verbs are keywords.

| Keyword                   | Function             | Example              |
|---------------------------|----------------------|----------------------|
| `have`, `has`, `had`      | Perfect aspect       | "had logged-in"      |
| `be`, `is`, `are`, `been` | Copula               | "is valid"           |
| `must`, `may`, `should`   | Deontic              | "must insert"        |
| `will`                    | Future aspect        | "will insert"        |
| `is` PastPart             | Passive              | "is inserted"        |
| `has/have`  PastPart      | perfect active       | "has logged-in"      |
| `has/have been`  PastPart | perfect passive      | "has been inserted"  |
| `has/have been`  Adj      | perfect copula + Adj | "has been valid"     |
| `has/have been`  NP       | perfect copula + NP  | "has been a manager" |

Singular subjects take `has`/`is`; plural subjects take `have`/`are`.

### 1.11 Adjectives (noun modifiers)

Adjectives are content words defined in the lexicon.
Comparative and superlative adjective word-forms are defined by the lexicon.

Examples of adjectives are: `valid`, `large`, `running`, `good`, `efficient`, `high-definition`.

### 1.12 Adverbs

Adverbs modify the closest preceding or following verb.
Adverbs are content words defined in the lexicon.

Examples:
 - "The signal clearly shows the status."  ("clearly" is attached to "show")
 - "The signal shows the status clearly."  ("clearly" is attached to "show") 

Examples of adverbs are: `quickly`, `well`, `badly`, `fast`, `far`.

### 1.13 Relative Clauses

Relative clauses modify the closest preceding noun.

The keyword `that` introduces a relative clause when it appears immediately after a noun phrase.
The keyword `that` introduces a complement clause when it appears immediately after a verb phrase.

Relative clauses MUST use `that`. The keywords `who` and `which` are forbidden.

Examples:
- "A customer that is verified inserts a card."
- "A card that has a valid code is accepted."
- "An account that has some administrative privileges and that is not used for two days." (Both clauses refer to the account).
- "A phone that has a cover that is broken." (the cover is broken)
  
### 1.14 Prepositional Phrases

Prepositional phrases typically attach to the verb.

Allowed prepositions are: 'in', 'on', 'at', 'under', 'above', 'below', 'beside', 'between', 'before', 'after', 'during', 'until', 'with', 'from', 'by', 'to', 'for', 'as', 'through'.

Examples:
 - "A clerk sends a letter to a manager." ("to a manager" attaches to "sends")
 - "A camera monitors the valve with a micro-lens." (The monitoring is done using a micro-lens)

 Use guillemets "« ... »" (or "<< ... >>") to explicitly attach prepositional phrases to noun phrases. 
 The determiner can be inside or outside the guillemets, there is no difference.
 To include a guillemet character inside a guillemet phrase, it must be escaped with a backslash: `\«`, `\»`, `\<<`, or `\>>`.

Examples:
 - "A clerk sends a letter to a manager." (the letter is sent to a manager)
 - "A clerk sends «a letter to a manager»." (the letter is addressed to a manager)
 - "A camera monitors the valve with a micro-lens." (The monitoring is done using a micro-lens)
 - "A camera monitors «the valve with a micro-lens»." (The monitored valve has a micro-lens)
 - "A camera monitors <\<the valve with a micro-lens>>." (The monitored valve has a micro-lens)

A guillemet-grouped phrase `« ... »` inherits the grammatical features (number, gender) of its head noun. The head noun is the first noun phrase within the guillemets. Internal modifiers do not affect the grammatical features of the grouped phrase.

Examples:
- "«A cat with nine tails» runs." (singular, head is "cat")
- "«The files in the archive» are compressed." (plural, head is "files")

Pronouns refer to the leading noun in a guillemet phrase:

 - "John rents a house for a vacation. It is nice." (The vacation is nice.)
 - "John rents a «house for a vacation». It is nice." (The house is nice.)

Nouns within the Guillemet Phrase are to be mentioned explicitly:
 - "A branch with a <\<leaf with a caterpillar>>. It is green." (The leaf is green) 
 - "A branch with a <\<leaf with a caterpillar>>. The caterpillar is green." (Explicit reference) 

### 1.15 Of-Clause

The preposition `of` always attaches to the immediately preceding noun phrase.

Examples:
 - "Every employee of the department attends."
 - "The CEO of the company resigns."
 - "Alice sees the color of the roof of the building. It is ugly." (The building is ugly.)
 - "Alice sees the color of the «roof of the building». It is ugly." (The roof is ugly.)
 - "Alice sees the «color of the roof of the building». It is ugly." (The color is ugly.)

### 1.16 String Literals

String literals appear as values in comparisons, file names, or with explicit noun phrases.
Pronouns don't refer back to string literals.
Strings are enclosed in double quotes: `"`, or within backticks `` ` ``. or within `⸄` and `⸅` .
Strings can contain arbitrary text, the only constraint on strings content is that delimiters must be escaped with a backslash: ` \ `. The backslash itself, when intended as a character of the string, must be escaped: "\\\\".

Examples:
 - If the status is "active" then the system proceeds.
 - The configuration is stored in the file "config.json".
 - The message "Missing '\"' at line ..." will appear.

### 1.17 Lists
  A list is a sequence of NP separated by commas and enclosed betwenn `:(` and `).
  A list can't be the antecedent of any pronoun.

The character `:` is structural only in these forms:
  - in the list opener `:(` (this section)
  - at the end of a line that immediately precedes a Markdown list (subject-scoped lists; VP coordination section)

If `:` does not appear in one of these forms then it has no SngL syntactic meaning.

#### Closed-World list (Exhaustive)
 To enumerate ALL the elements of a list or set.

 - "There can only be three states: (idle, running, stopped. )"
 - "The only valid priorities are: (low, medium, high. )"
 - "There are system can be: (idle, running, waiting. )"
 - "All users must log-in once a week excluding: (the system-administrator, the root-user, the db-admin)."

#### Open-World list (Extensible)
 To provide examples.
 - "The known vulnerabilities include: (SQL-injection, XSS.)"
 - "The data will be stored in markdow file, for example: (pippo.md, pluto.md.)"

There might be zero or any number of spaces between ":" and "(". 

Note: parenthesis are not allowed anywhere else.

 - "The user (normally a cat) will press the button." WRONG. Incidental sentences must be rephrased.
 - "The user, that is normally a cat, will press the button." CORRECT.

Except for strings and numeric expressions that can contain any character.
 - "The function `f(s)` is monotonic."
 - "The expression {3*(2+x)} is non-zero."

### 1.18 Annotations

Annotations are metadata enclosed in double brackets or '〚 ... 〛'.
Annotations can appear in any position a blank can appear and do not affect the sentence meaning.
Annotations may contain any character sequence except the closing delimiter `]]` or `〛`. They are treated as whitespace by the parser.

Examples:
 - "The core must be closed. [[ requested-by: Security team ]]"
 - "No cat is under the table. 〚 camera: cam-1 〛"

### 1.19 Mass Nouns (Uncountable Substances)

Mass nouns denote substances or aggregates that are not individuated into discrete units.

Mass nouns are content words defined in the lexicon.
Examples of mass nouns are: `water`, `fuel`, `data`, `memory`, `bandwidth`, `disk-space`.

#### Grammatical Behavior

A mass noun phrase (determiner + mass noun) always has number `s` and gender `n`.

 - "The water **is** clean. **It** flows."
 - "Some data **is** corrupted. **It** must be deleted."

#### Allowed Determiners

| Determiner | Example |
|------------|---------|
| `the`, `this` | "The data is encrypted." |
| `some` | "Some water leaks." |
| `no` | "No bandwidth is available." |
| `more`, `less` | "More memory is allocated." |
| `enough` | "Enough storage is provisioned." |

Forbidden: `a/an`, `every`, `each`, cardinal numbers.

Mass noun phrases may also appear as complements of proportion partitives (Section 1.19.2.1):
  - "Some of the water evaporates."
  - "Half of the fuel is consumed."
  - "30% of the data is corrupted."

#### Dual Mass/Count Nouns

Some words have both mass and count entries. The determiner disambiguates:

For example, `coffee` can be a mass noun or a count noun.

Examples:
 - "Some coffee is spilled." (mass: substance)
 - "A coffee is ordered." (count: serving)

### 1.19.1 Quantified Mass (Measurement Phrases)

A measurement phrase makes a mass noun countable:

```
NUM UNIT of MassNoun
```

Quantified masses follow standard noun rules with number determined by NUM.

Examples:
 - "1 liter of water **is** added. **It** is warm."
 - "Three liters of water **are** added. **They** are warm."
 - "0.5 liters of data **are** cached. **They** must be flushed."

#### Units

Units are lexicon entries:

Units are content words defined in the lexicon.
Examples of units are: `liter`, `MB`, `kilogram`, `joule`.

### 1.19.2 Partitive Constructions

Partitives select a subset of a known, definite quantity or set.

SngL supports two families of partitives:
  - **Proportion partitives**: select a portion of a mass quantity or a portion of a plural count set.
  - **Count partitives**: select an exact (or bounded) number of elements from a definite plural count set.

### 1.19.2.1 Proportion Partitives (Mass or Plural Count)

Proportion partitives:

| Partitive | Example |
|-----------|---------|
| `some of the` | "Some of the water evaporates." |
| `all of the` | "All of the data is backed up." |
| `none of the` | "None of the fuel is consumed." |
| `most of the` | "Most of the memory is allocated." |
| `half of the` | "Half of the storage is reserved." |
| `N% of the` | "30% of the data is corrupted." |

The complement after `of the` MUST be either:
  - a mass noun phrase, or
  - a plural count noun phrase.

Grammatical behavior:
  - If the complement is a mass noun phrase, the result is a singular mass NP (singular agreement; `it/its/itself`).
  - If the complement is a plural count noun phrase, the result is a plural count NP (plural agreement; `they/them/their/themselves`).

Examples:
  - "Half of the water evaporates."
  - "Half of the cards are damaged."
  - "30% of the data is corrupted."
  - "30% of the boxes are damaged."

**Key distinction:**
  - "Some water leaks." — existential: there exists water that leaks
  - "Some of the water leaks." — partitive: a portion of the (known) water leaks
  - "Some of the cards are valid." — partitive over a plural count set

### 1.19.2.2 Count Partitive Constructions

Count partitives select a subset of a known, definite plural set of countable entities.

| Form                              | Example                                          |
|-----------------------------------|--------------------------------------------------|
| `one of the` PluralNP             | "The operator may push one of the buttons."      |
| `only one of the` PluralNP        | "The operator may push only one of the buttons." |
| `N of the` PluralNP               | "2 of the buttons are pressed."                  |
| `exactly N of the` PluralNP       | "Exactly 2 of the buttons are pressed."          |
| `only N of the` PluralNP          | "Only 2 of the buttons are pressed."             |
| `at least N of the` PluralNP      | "At least 2 of the buttons are pressed."         |
| `at most N of the` PluralNP       | "At most 2 of the buttons are pressed."          |
| `between N and M of the` PluralNP | "Between 3 and 5 of the LEDs blink."             |

Exact-count forms:
  - `N of the PluralNP` means `exactly N of the PluralNP`.
  - `only` and `exactly` are optional markers for exact-count partitives; they do not change the meaning.

where `PluralNP` MUST be a definite plural count noun phrase headed by a `noun` entry (or a plural `unique_noun` entry).

Grammatical behavior:
  - The resulting noun phrase inherits **gender** from the head noun of `PluralNP`.
  - **Agreement**: if the selected count is 1 then the result is singular; otherwise the result is plural.

### 1.19.3 Mass Comparisons

| Construction                | Example                                        |
|-----------------------------|------------------------------------------------|
| `more` MassNP `than` MassNP | "The tank contains more water than fuel."      |
| `less` MassNP `than` MassNP | "The process uses less memory than bandwidth." |
| `the amount of` MassNP      | "The amount of data is greater than 5 GB."     |

## 2. Present Perfect (Prior Events)

The present perfect expresses that a predicate held at some time-point prior to the evaluation point.
It is permitted **only** in:
- Conditional antecedents (`if`, `whenever`, `once`, `each time`, `while`, `until`, `unless`, `except when`)
- Relative clauses (introduced by `that`)

Present perfect is **not permitted** in main clauses, consequent clauses, deontic statements, or future temporal statements.

The present perfect asserts existence of a prior time-point at which the predicate held:

- "If a card **has been inserted**" — ∃t < t₀ : inserted(card, t)
- "If the system **has never failed**" — ¬∃t < t₀ : failed(system, t)

It does **not** assert that the state persists. Compare:
- "A customer that **has logged-in**" — logged in at some past time (may or may not still be logged in)
- "A customer that **is logged-in**" — currently logged in

Note: SngL does not use simple past tense forms to express prior events. Use present perfect where it is syntactically permitted.

### 2.1 Positive Forms

| Pattern | Example |
|---------|---------|
| NP `has/have` PastPart (NP) | "...whenever a customer has logged-in..." |
| NP `has/have been` NP | "...that has been a manager..." |
| NP `has/have been` Adj | "...if the card has been valid..." |
| NP `has/have been` PastPart | "...if the card has been inserted..." |

### 2.2 Negation forms

| Pattern | Example |
|---------|---------|
| NP `has/have not` PastPart | "...if the system has not responded..." |
| NP `has/have not been` Predicate | "...that has not been authorized..." |
| NP `has/have never` PastPart | "...if the account has never received a deposit..." |
| NP `has/have never been` Predicate | "...that has never been an administrator..." |

Note: `never` asserts that no prior time-point exists at which the predicate held (¬∃t < t₀).

**Agreement:** Singular subjects take `has`; plural subjects take `have`.

### 2.3 Examples

Relative clauses:
- "A customer that **has been verified** may withdraw funds."
- "A request that **has failed** three times is escalated."
- "A user that **has never logged-in** is marked as inactive."

Conditional antecedents:
- "If a session **has expired** then the user must re-authenticate."
- "Whenever a backup **has completed**, the system sends a notification."
- "Once the payment **has been confirmed**, the order is dispatched."
- "The system retries until the request **has succeeded**."


## 3. Predicates (Truth values)

### 3.1 Properties/actions

| Pattern                     | Example                                    |
|-----------------------------|--------------------------------------------|
| NP `is/are` NP              | "John is a manager."                       |
| NP `is/are` Adjective       | "The card is valid."                       |
| NP `is/are` PastParticiple  | "The card is inserted."                    |
| NP VP                       | "A customer inserts a card."               |
| `there is/are` NP           | "There are two customers."                 |
| `there is/are` NP RelClause | "There is a customer that has no account." |

Note: Exceptions using `except when` are specified in Section 4.

### 3.2 Negation

 - NP `does/do not` VP — "A customer does not insert a card."
 - NP `is/are not` Predicate — "The system is not ready."
 - `no` NP VP — "No customer inserts a card."
 - `no` Adj* NP VP — "No banned customer inserts a card."

### 3.3 Sentence Negation

 - `it is false that` S — "It is false that a customer inserts a card."

### 3.4 Comparisons

#### 3.4.1 Comparative constructions (adjectives):

 - NP `is` ComparativeAdj `than` NP — "Tank A is larger than Tank B."
 - NP `is` ComparativeAdj `than` NUM — "The temperature is greater than 100."
 - NP `is` ComparativeAdj `than` NumExpr — "The count is less than {N + 5}."

#### 3.4.2 Comparative constructions (adverbs):

 - NP VP ComparativeAdv `than` NP — "Process A completes more-quickly than Process B."
 - NP VP ComparativeAdv `than` NP VP — "The server responds faster than the client requests."

#### 3.4.3 Superlative constructions (adjectives):

 - NP `is the` SuperlativeAdj — "Tank A is the largest."
 - NP `is the` SuperlativeAdj `of` NP — "Tank A is the largest of the three tanks."

Superlative constructions (adverbs):

 - NP VP `the` SuperlativeAdv — "Process A completes the most-quickly."
 - NP VP `the` SuperlativeAdv `of` NP — "Process A completes the fastest of all processes."

Equality and range:

 - NP `is equal to` NUM/NP — "The priority is equal to 3."
 - NP `is not equal to` NUM/NP — "The status is not equal to "pending"."
 - NP `is between` NUM `and` NUM — "The response-time is between 10 and 50 milliseconds."

Note: `greater`, `less`, `greatest`, `least`, `faster`, etc. are comparative and superlative forms defined in the lexicon, not special syntax.

Note: In comparative constructions, `than` is a syntactic marker introducing the comparand, not a preposition. In superlative constructions with `of`, the `of` introduces the comparison domain.

Disambiguation: `less` and `more` followed by `than` are comparatives ("The count is less than 5", "Process A runs more quickly than Process B"). `less` and `more` followed by a mass noun are determiners ("Less water is available", "More memory is allocated").


### Purpose Clauses (Rationale/Purpose)

SngL supports optional purpose clauses introduced by the fixed keyword phrase `in order to`.
A purpose clause is non-normative: it does not change the formal-logic mapping of the sentence, but tools MAY retain it for later analysis.

The comma delimiting is mandatory.

Forms:

 - `In order to` PurposeVP `,` S.
 - S `, in order to` PurposeVP `.`
 - S1 `, in order to` PurposeVP `,` S2.

Rules:

 - PurposeVP MUST be an infinitive VP (base verb form) with no explicit subject.
 - The purpose clause MUST NOT contain a comma token.
 - The purpose clause ends at the delimiting comma (incidental form) or the final period (suffix form).
 - The purpose clause MUST NOT be used to state requirements; use it only to express intent/purpose/rationale.

Examples:

 - "In order to comply with the current laws, the lever must be locked."
 - "The system must log all operations, in order to ensure that any transaction is auditable."
 - "The system must log all operations, in order to ensure auditability, and it must store the logs for 90 days."

## 4. Conditional Statements

A conditional statement combines two predicates: a **condition** (antecedent/trigger) and a **consequence**.
Both the condition and the consequence are predicates (Section 3).

If-Then Conditionals

* `if` Condition `then` Consequence — "If a card is valid then the system accepts the card."
* `if` Condition `then` Consequence₁ `, otherwise` Consequence₂ — "If a card is valid then the system accepts it, otherwise the system will reject the card."
* `if` Condition, Consequence — "If a card is valid, the system accepts the card."

Reactive Conditionals

* `whenever` Condition `then` Consequence — "Whenever a request arrives then the system logs it."
* `whenever` Condition, Consequence — "Whenever a request arrives, the system logs it."

Trigger Conditionals

* `once` Trigger `then` Consequence — "Once the client is authorized then yey can start operating."
* `once` Trigger, Consequence — "Once triggered, the switch will return to its position."
* `each time` Trigger `then` Consequence — "Each time the lever is pulled then it must be released."

Exceptions

* Action, `except when` Condition — "The system logs every access, except when in recover mode."

Other conditional connectives that scope future behavior include `before`, `after`, `until`, `unless`, and `while` (see Section 6).

### Tense and Aspect in Conditionals

The condition (antecedent/trigger) is evaluated as a test. It MUST use present tense (current-state test) or present perfect (prior-event test; Section 2).

SngL does not use simple past tense forms in conditions. Do not use `was/were` or bare past verb forms to express a prior event; use present perfect where permitted.

Condition/trigger tenses:

| Tense/aspect | Test | Example |
|-------------|------|---------|
| Present | Current state | "If the card **is** valid then..." |
| Present perfect | Prior event | "If the card **has been validated** then..." |

Consequent clauses use present tense or future tense:

| Tense | Semantics | Example |
|-------|-----------|---------|
| Present | State or fact (per Section 8.3) | "If X has occurred then the system **logs** it." |
| Future | Behavior (per Section 8.3) | "If X has occurred then the system **will** retry." |

Note: Present perfect is forbidden in consequents (Section 2). SngL does not use simple past tense forms in consequents; use present tense or future tense.

## 5. Deontic Statements (Obligations, Permissions, Prohibitions, Capabilities)

| SngL | Deontic | Example |
|------|----------|--------|
| NP `must` VP | obligation | "Every user must authenticate." |
| NP `must not` VP | prohibition | "The system must not store a plaintext password." |
| NP `may` VP | permission | "A user may export yeir data." |
| NP `may preferably` VP | permission (but expected) | "A user may preferably use two-factor authentication." |
| NP `preferably may` VP | permission (but expected) | "A user preferably may use two-factor authentication." |
| NP `should` VP | obligation (but there might be exceptions) | "The system should log all debug events." |
| NP `is expected to` VP | obligation (but there might be exceptions) | "The system is expected to compress all old log-files." |
| NP `is able to` VP | capability | "The operator is able to restart the server." |


## 6. Temporal Statements (Future Tense)

### 6.1 Basic Temporal

| SngL                     | Example                                        |
|--------------------------|------------------------------------------------|
| NP `will always` VP      | "The system will always be available."         |
| NP `will eventually` VP  | "Every request will eventually be processed."  |
| NP `will immediately` VP | "The counter will immediately be incremented." |
| NP `will then` VP        | "The system will then validate the input."     |
| NP `will never` VP       | "The system will never be in an error-state."  |
| NP `will not` VP         | "The system will not respond."                 |

Note: Time is discrete, so "immediately" means: "at the next possible step".

### 6.2 Temporal with Scopes

| SngL                                                   | Example                                                          |
|--------------------------------------------------------|------------------------------------------------------------------|
| `before` Condition, NP `will` VP                       | "Before initialization, the system will wait."                   |
| `after` Condition, NP `will` VP                        | "After initialization, the system will always be operational."   |
| `after` Condition `and before` Condition, NP `will` VP | "After startup and before shutdown, the system will log all events." |

### 6.3 Conditional Time Continuous

| SngL                            | Example                                                      |
|---------------------------------|--------------------------------------------------------------|
| NP `will` VP `until` Condition  | "The wheel will turn until the lever is engaged."            |
| NP `will` VP `unless` Condition | "The server will run unless a shutdown-command is received." |
| `while` Condition, NP `will` VP | "While the system is active, it will monitor processes."     |

### 6.4 Temporal Sequences

| SngL                             | Example                                                                                                 |
| ---------------------------------|---------------------------------------------------------------------------------------------------------|
| `first` S, `then` S              | "First a customer enters a card, then the system verifies it."                                          |
| `first` S, `then` S, `finally` S | "First a customer enters a card, then the system verifies it, finally the customer receives a receipt." |

Note: There can be any number of ", then S"

### 6.5 Temporal with Bounds

| SngL                                          | Example                                            |
|-----------------------------------------------| ---------------------------------------------------|
| NP `will` VP `within` Duration                | "The system will respond within 30 seconds."       |
| NP `will` VP `for` Duration                   | "The session will remain active for 30 minutes."   |
| NP `will` VP `every` Duration                 | "The heartbeat is sent every 30 seconds."          |
| NP `will` VP `at least every` Duration        | "The heartbeat is sent at least every 30 seconds." |
| NP `will` VP `at most every` Duration         | "The sensor reports at most every 5 minutes."      |
| NP `will` VP `every` Duration₁ `to` Duration₂ | "The sensor reports every 1 to 3 minutes."         |


Duration Units
 - `nanosecond` / `nanoseconds` / `ns`
 - `millisecond` / `milliseconds` / `ms`
 - `second` / `seconds` / `sec`
 - `minute` / `minutes` / `min`
 - `hour` / `hours` / `hr`
 - `day` / `days`
 - `week` / `weeks`
 - `month` / `months`
 - `quarter` / `quarters`
 - `year` / `years` / `yr`

## 7. Coordination

The English conjunctions `and`, `but`, `whereas`, and `or`,  are reserved for sentence coordination and must never be used with NP, Adjectives, Adverbs, or VP.

Coordination operators are left associative but some operator has higher precedence than others.

### 7.1 Noun phrases, adjectives, and adverbs Coordination

Noun phrases, Adjectives, Adverbs, and Verb Phrases are coordinated by special operators.

| Op. | Meaning             |
|-----|---------------------|
| `&` | conjunction ("both") |
| `/` | exclusive disjunction ("one or the other but not both) |
|`&/` | inclusive disjunction ("one or the other but also both) |

No other English word or operator can be used for Noun phrases, adjective, or adverb coordination.

#### 7.1.1 Conjunction

A NP conjunction produces another NP which is always plural.

Examples:
 - "John & the dog run." (They do it together)
 - "John & the dog run. They are happy." (Both of them are happy)
 - "<<A striped cat>> & the dog run." (Two animals running together)
 - "A card & pin is provided to employees." (Determiner extends over the second NP, if missing).
 - "A red & green sticker." (The sticker has both colors on it)
 - "The cord rewinds slowly & steadily." (The cord rewinds in a smooth fashion)
 
#### 7.1.2 Exclusive Disjunction 

A NP exclusive disjunction produces another NP which is always singular. 
If all the NPs have the same gender, the new NP is of the same gender, otherwise it is of "common" gender.

Examples:
 - "The operator can use a hammer/mallet." (can use one or the other but not both at the same time)
 - "<<A red striped cat>> / <<the dog>> runs." (use guillemets to explicitly bind determiners )
 - "A card / pin is provided to employees." (Determiner extends over the second NP, if missing).
 - "A plastic / ceramic container." (The container is entirely of plastic or entirely of ceramic)
 - "The operator can use a hammer/plier. It is owned by the Company." (The company owns the tool which is used)
  - "John / <<the dog>> runs. Yey is happy." (whoever runs is happy)
 - "A manager/clerk approves the request. The manager signs the request; the clerk stamps it." (Explicit reference with "the" is allowed)

#### 7.1.3 inclusive Disjunction 

A NP inclusive disjunction produces another NP which is always plural. 

Examples:
 - "A <<red led>> &/ a <<green led>> blink." (one or both of the leds blink)
 - "<<The red led>> &/ <<A warning message>> blink. They must be turned off." (what blinks, must be turned off)
 - "<<The red led>> &/ <<A warning message>> blink. They must be turned off." (what blinks, must be turned off)

#### 7.1.4 Quantification over coordination

If the quantifier or determiner of the second NP is missing, the first quantifier applies at the coordinated NP as a whole.

Examples:
 - "The operator has three mallets/hammers." (The operator has three tools each one can be a hammer or a mallet)
 - "The operator has <<three mallets>> / <<three hammers>>."  (The operator has three tools all of the same type)
 - "A card/badge is not provided to visitors." (Visitors can't have a card and can't have a badge)
 - "Two <<men with an hat>>/<<black dogs>> are on the hill." (Two entities are on the hill, each one can be a man or a dog)
 - "<<Two men with an hat>>/<<Three black dogs>> are on the hill." (Two men are on the hill or three dogs are on the hill)

#### 7.1.5 Negation over coordination

Negation is a zero quantifier.

Example:
 - "No card/badge is provided to visitors." (Visitors can't have a card and can't have a badge)

#### 7.1.6 Multiple coordination
Only coordination of the same type are allowed.
Examples:
 - "The flag is white & red & blu."   (All colors are present)
 - "The flag is white / red / blu."   (Only one color is present)
 - "The flag is white &/ red &/ blu." (Any combination of colors is allowed)

Note: For more complex coordination, use Verb Phrase or sentence coordination.

#### 7.1.7 Examples

 - "Alice owns a car / a bike."  (Alice has one vehicle that can be a car or can be a bike)
 - "Alice owns a car & a bike."  (Alice has two vehicles; one is car, the other is a bike)
 - "Alice owns a car &/ a bike." (Alice has one or two vehicles::a car, a bike, or both)
 - "The flag is red / blue."  (The flag is all red or all blue)
 - "The flag is red & blue."  (The flag has both colors)
 - "The flag is red &/ blue." (The flag can be all red, all blue, or have both colors on it)

### 7.2 Verb Phrase Coordination

Verb phrases that share the same NP can be coordinated using special operators.

| Operators                  | Meaning                                         |
|----------------------------|-------------------------------------------------|
| NP VP₁ `&&` VP₂            | Conjunction (both VP₁ and VP₂)                  |
| NP VP₁ `&& then` VP₂       | Sequential (VP₂ only after VP₁)                 |
| NP VP₁ `&&/` VP₂           | Inclusive disjunction (VP₁ or VP₂ or both)      |
| NP `either` VP₁ `or` VP₂   | Exclusive disjunction (VP₁ or VP₂ but not both) |
| NP `neither` VP₁ `nor` VP₂ | Negative conjunction (not VP₁ and not VP₂)      |

Each VP must have its own complete argument structure:
 - NO: "The operator inserts && validates the card." (Invalid, no object in the first VP)
 - OK: "The operator inserts the card && validates it." (Correct, `it` refers to `the card`)

No other English word or operator can be used for Verb Phrase coordination.
In Verb Phrase coordination, the NP always precedes the keywords `either` and `neither`.

#### 7.2.1 Conjunction

Examples:
 - "The operator inserts the card && pulls the lever." (Both actions occur in an unspecified temporal order)
 - "The operator inserts the card && then pulls the lever." (card insertion is completed before pulling the lever)
 - "The system authenticates the user && then grants the user an access." (Access is granted only after authentication)

#### 7.2.2 Sequential Conjunction

The operator `&& then` expresses sequential coordination: VP₂ begins only after VP₁ completes.

- When an operator scopes over the entire coordination, both VPs share that operator, with VP₂ sequenced after VP₁.
- When guillemets restrict operators to individual VPs, each VP has its own operator, but VP₂ still occurs after VP₁ completes.

Examples:
- "The system will validate && then log." → Both future, logging after validation
- "The operator must insert && then pull." → Both obligatory, pulling after insertion
- "The operator <<must insert>> && then <<may pull>>." → Insertion obligatory; after insertion, pulling permitted
  
#### 7.2.3 Negative Conjunction
The sentence "NP `neither` VP₁ `nor` VP₂" means none of VP₁ and VP₂ holds for the subject NP (they are both false).
Examples:
 - "The alarm neither blares nor flashes." (The alarm produces no sound and no light)

Note: The NP precedes the keyword `neither`.

#### 7.2.4 Inclusive Disjunction

Inclusive disjunction between two verb phrases which share the same subject NP is expressed with `&&/`.

Examples:
 - "The alarm-signal blares &&/ flashes." (The alarm can blare, flash, or do both at the same time)
 - "A customer buys a fork &&/ uses yeir tools." (The customer can buy a fork and also use their own tools )
 
#### 7.2.5 Exclusive Disjunction

The sentence "NP `either` VP₁ `or` VP₂" means that exactly one of VP₁ and VP₂ holds for the subject NP.
 - "Either the alarm-signal blares or flashes." (The alarm can blare or flash but can't do both at the same time)

Note: The NP precedes the keyword `either`.

#### 7.2.6 Adverb Scope in VP Coordination

Adverb scope is determined by position:
- An adverb before the first verb (including sentence-initial position) scopes over the entire coordinated VP.
- An adverb after a verb scopes over that VP only.

Examples:
- "Slowly the operator turns the knob && pulls the lever." → both slow
- "The operator slowly turns the knob && pulls the lever." → both slow
- "The operator turns the knob slowly && pulls the lever." → only turning is slow
- "The operator turns the knob && carefully pulls the lever." → only pulling is careful

#### 7.2.7 Deontical, Temporal and Negation  Scope in VP Coordination

Deontic operators (`must`, `may`, `should`, etc.), temporal operators (`will`, `will always`, `will eventually`, etc.), and negation (`does not`, `is not`) scope over the entire coordinated VP by default.

To restrict an operator to a single VP, use guillemets to group the operator with its VP:
- "The operator must insert the card && pull the lever." → Both obligatory
- "The operator <<must insert the card>> && pulls the lever." → Insertion obligatory; pulling is factual
- "The operator does not insert the card && pull the lever." → Neither occurs
- "The operator <<does not insert the card>> && pulls the lever." → No insertion; pulling occurs
- "The operator <<must insert the card>> && <<may pull the lever>>." → Insertion obligatory; pulling permitted

#### 7.2.8 Multiple VP coordination.

Coordination operators of the same type are left associative but some operator has higher precedence than others.

The conjuction `&&` binds tighter than `&& then`.
The conjuction `&& then` binds tigher than the disjunction `&&/`.
The keyword `or` is bound to the closest preceding keyword `either`.
The keyword `nor` is bound to the closest preceding keyword `neither`.

For the infix operators this is the precedence order: `&&` > `&& then` > `&&/`. 

 - "X P && Q && then S." -> "X <<P && Q>> && then S." (S depends on both P and Q)
 - "X P && then Q && S." -> "X P && then <<Q && S>>." (both S and Q depend on P)

 - "X P &&/ Q && then S." -> "X P &&/ <<Q && then S>>." (P or (Q followed by S), or both)
 - "X P && then Q &&/ S." -> "X <<P && then Q>> &&/ S." ((P follwed by Q) or S), or both)

Use guillemets for overriding the precedence order.

 - "X P && <<Q && then S.>>"                          (S depends only on Q)
 - "X <<P && then Q>> && S."                          (Q depends on P; S is independent)

Examples:
 - "The dog barks && cries && then bites."     (The dog only bites after barking and crying)
 - "The dog barks && then cries && bites."     (The dog bites and cries only after barking)
 - "The dog <<barks && then cries>> && bites." (The dog cries only after barking but bites at any time)
 - "The dog barks && <<cries && then bites.>>" (The dog bites only after crying but barks at any time)
 - "The dog barks &&/ cries && then bites."    (The dog barks, (cries and bites), or does both)
 - "The dog barks && then cries &&/ bites."    (The dog (barks and cries), bites, or does both)
 - 
 - "The lever either is locked && is in the off-position or is in the on-position." (The lever is )
 - "The system either logs all access-data && archives them or deletes them." (Exclusive)

Invalid forms:
 - "The lever is locked or is in the on-position." (Invalid — `or` is inclusive at the sentence level; use `either ... or ...` for exclusive)
 - "The lever is locked / is in the on-position." (Invalid — `/` is an NP coordination operator, not a VP coordination operator)
 - "The lever either is locked or is in the off-position or is in the on-position." (Invalid — VP disjunction must have exactly two disjuncts; use guillemets for grouping or split into multiple sentences)

#### 7.2.9 List coordination 

If the same NP is the subject of multiple verb phrases coordination, and all coordinations are of the same type (`&&` or `&& then`) then:

 - If there are **exactly two** verb phrases then `&&` (or `&& then`) MUST be used.
 - If there are **three or more** verb phrases then a subject-scoped list must be used.

A subject-scoped list is introduced by a Noun phrase that ends with a structural character `:`.
The character `:` MUST be the last non-space character of the line. The next non-blank line MUST begin a Markdown list.

Example (conjunction):

```sngl
Each finding:
  - includes a severity.
  - includes a rule-id.
  - should include a location.
```

Example (sequential conjunction):

```sngl
The player then: 
  - moves three steps.
  - pushes the button.
  - opens the bottle.
```

This is interpreted as if the NP was repeated for each VP:

 - "Each finding must include a severity and each finding must include a rule-id and each finding should include a location."
 - "The player must move three steps and then the player pushes the button and then the player opens the bottle."

Example (exclusive disjunction):

```sngl
The player either:
  - moves three steps.
  - pushes the button.
  - opens the bottle.
```

### 7.3 Sentence-Level Coordination

| Operators             | Meaning                                     |
|-----------------------|---------------------------------------------|
| S `, and` Q           | Conjunction (both S and Q)                  |
| S `, or` Q            | Inclusive disjunction (S or Q or both)      |
| `either` S `, or` Q   | Exclusive disjunction (S or Q but not both) |
| `neither` S `, nor` Q | Negative conjunction (not S and not Q)      |

#### 7.3.1 Conjunction

The conjunction of two sentences asserts that both sentences are true and is expressed with `and`, `but`, oor `whereas`.

 - "A boy runs, **and** a dog chases him." (Both sentences are true)
 - "A feather is light, **whereas** a brick is heavy." (Both sentences are true, "whereas" is a contrastive version of "and")
 - "A laser is expensive, **but** it is more precise." (Both sentences are true, "but" is a contrastive version of "and")

#### 7.3.2 Negative conjunction

The negative conjunction asserts that both sentences are false.

 - "**Neither** the light blinks **nor** the bell rings." (Both are false)

Note: The NP follows the keyword `neither`.

#### 7.3.3 Inclusive disjunction

 The inclusive disjunction of two sentences asserts that one of the two, or both, are true:

 - "The light blinks, **or** the bell rings." (One of the two is true but both can alse be true)
  
#### 7.3.4 Exclusive disjunction

 The exclusive disjunction of two sentences asserts that only one of the two is true.
 
 - "**Either** the daily menu is available, **or** a bento is provided." (Only one of the two is true)

Note: The NP follows the keyword `either`.


## 8. Interpretation Rules

This section specifies deterministic disambiguation rules and semantic defaults.

### 8.1 Reference Resolution

  - Definite reference: `the X`, `this X`, and `these Xs` MUST refer to the most recent, most specific accessible noun phrase matching X.
  - If no matching antecedent exists:
    - for `unique_noun` heads: introduce a new discourse referent with a uniqueness presupposition
    - otherwise: treat `the X` as `a X` (singular) or treat `the Xs` as `some Xs` (plural)
  - Proper noun uniqueness: within a document, each `proper_noun` form MUST refer to exactly one discourse referent.
  - Unique noun uniqueness: within a uniqueness scope, each `unique_noun` head MUST refer to exactly one discourse referent.
  - Uniqueness scope: an `of`-clause introduces a uniqueness scope for a `unique_noun` head (per Section 1.5).
  - Named references: named entities introduced via Binding (Section 1.8) MUST refer to their binding when one exists.
  - Pronoun matching: pronoun resolution requires matching by number and gender. Antecedent features come from the lexicon; pronoun features are fixed by the pronoun keyword list. Gender matching: `he/him/his/himself` match `m`; `she/her/hers/herself` match `f`; `yey/yem/yeir/yeirs/yemself` match any non-neuter singular (`m|f|c`); `it/its/itself` match `n`; plural pronouns match number `p` and ignore gender.
  - Non-reflexive pronouns and possessive determiners MUST refer to the most recent matching antecedent excluding the subject of the current sentence.
  - Tie-breaking: if multiple equally-good antecedents exist, choose the most recent subject; if none, choose the most recent object; if still tied, choose the most recent mention. (This makes reference resolution deterministic.)
  - Reflexive pronouns and reflexive possessives MUST refer to the subject of the current sentence.
  - Opaque values: string literals, numeric expressions, and annotations do not introduce discourse referents and cannot be antecedents for pronouns or definites.
  - Guillemets and antecedents: a guillemet-grouped noun phrase introduces a primary discourse referent corresponding to its head noun phrase; for nested guillemets, the outermost head is the primary antecedent.

### 8.2 Attachment Rules

  - The preposition `of` MUST attach to the immediately preceding noun phrase. 
  - Prepositional phrases outside guillemets MUST attach to the verb.
  - Adjectives MUST modify only the immediately following noun.
  - Adverbs MUST modify the closest verb: pre-verbal adverbs modify the following verb; post-verbal adverbs modify the preceding verb.
  - Coordination within a prepositional phrase MUST apply to the complement noun phrases, not to the entire PP or the sentence subject.
  - Disambiguation: when coordinated NPs within a PP have different semantic types, the coordination applies at a higher level.

### 8.3 Scope and Precedence

  - Determiner scope follows textual order (left-to-right). The first quantifier mentioned has widest scope; later quantifiers are nested within earlier ones.
  - `not` (VP negation) scopes over the verb phrase.
  - `no` (NP negation) scopes over the noun phrase it heads.
  - Due to textual scope ordering, `Every X does not VP` means "no X does VP" (universal scopes over negation), which may be unintended.
  - Conjunction precedence: `and` and `but` bind tighter than `or`. A comma before a conjunction opens wider scope.
  - `a distinct`: when a noun phrase with determiner `a distinct` appears in the scope of a universal quantifier Q, the existential entity MUST be distinct for each binding of Q's variable. If multiple universal quantifiers are in scope, the distinctness constraint binds to the innermost one.
  - Tense: present-tense sentences describe states and facts (what is the case). Future-tense sentences describe behavior (what will happen over time).
  - VP coordination: `&&` distributes the subject to both VPs; `&& then` additionally enforces temporal ordering (VP2 begins after VP1 completes). VP coordination is left-associative.
  - Operator scope over coordinated VPs: deontic operators, temporal operators, and negation scope over the entire coordinated VP by default. Guillemets restrict operator scope to a single VP.
  - Adverb scope in coordinated VPs: an adverb before the first verb (including sentence-initial position) scopes over the entire coordinated VP. An adverb after a verb, or immediately before a verb following `&&`, scopes over that VP only.
  - In coordinated VPs, pronouns in later VPs can refer to antecedents introduced in earlier VPs.
  - Generic vs episodic (indefinite subjects): copula sentences like `A lion is a mammal` are generic/definitional (universal), while action verb sentences like `A lion enters the cage` are episodic (existential).
  - Tense restriction: present perfect is syntactically restricted to conditional antecedents and relative clauses. Main clauses use present tense (states/facts) or future tense (behavior). Consequent clauses use present or future tense.

### 8.4 Agreement Rules

  - Mass noun phrases take singular verb agreement and use `it/its/itself` pronouns.
  - In a measurement phrase `NUM UNIT of MassNoun`, the number controls agreement and pronoun selection. Any number other than 1 (including fractional numbers) takes plural agreement.
  - In a proportion partitive phrase (Section 1.19.2.1), agreement follows the complement: mass -> singular; plural count -> plural.
  - In a count partitive phrase (Section 1.19.2.2), agreement follows the selected count: 1 -> singular; otherwise -> plural.
  - Guillemets apply uniformly: a guillemet-grouped phrase inherits grammatical properties (number, gender) from its head noun.

### 8.5 Temporal Interpretation

  - Present tense predicates are evaluated at the current time-point t₀.
  - Present perfect predicates assert existence of a prior time-point t < t₀ at which the predicate held. The negated form (`has not`, `has never`) asserts no such time-point exists.
  - Future tense predicates assert existence of (or universal quantification over) later time-points t > t₀, depending on the temporal operator:
    - `will eventually` — ∃t > t₀
    - `will always` — ∀t > t₀
    - `will never` — ¬∃t > t₀ (equivalent to `will always not`)
  - In conditional statements, the antecedent is evaluated first; if true, the consequent is evaluated at the same or subsequent time-point depending on tense.
  - Present perfect in relative clauses restricts the noun phrase to entities for which the predicate held at some prior time.
  - Duration phrases in present perfect contexts (e.g., "has not been used **for 90 days**") specify the span of time-points during which the predicate held or failed to hold.

## 9. Ill-formed Constructions

The following constructions are explicitly forbidden in SngL to maintain ambiguity-free parsing:

1.  **Dangling modifiers**: Adjectives and adverbs must immediately follow/precede their target as specified in Attachment Rules.
2.  **Ambiguous coordination**: Coordination without explicit guillemets that violates the default left-associativity or scope rules is invalid if it leads to ambiguity.
3.  **Unknown words**: Any content word not defined in the lexicon or introduced via a literal/binding is an error.
4.  **Tense mismatch**: Using present perfect in main clauses or future statements is a syntax error.
5.  **Particle movement**: Phrasal verbs must be adjacent (`looks-up the word`, not `looks the word up`).

## Appendix A: Glossary

- **NP**: Noun Phrase
- **VP**: Verbal Phrase
- **Adj**: Adjective
- **Adv**: Adverb
- **RelClause**: Relative Clause
- **NUM**: Number
- **Predicate**: A sentence with a truth value (true/false)
- **Condition**: A predicate that influences the truth value of a sentence
- **Consequence**: A sentence that is influenced by the truth value of a predicate
- **Content word**: A word that carries meaning (a name, an object, a role, ...)
