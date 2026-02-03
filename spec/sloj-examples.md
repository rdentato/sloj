# Sloj Language Examples

**Version**: 0.1  
**Date**: 2026-02-01  
**Purpose**: Comprehensive examples demonstrating all Sloj language constructs

---

## Quick Reference Index

Search this table to quickly find examples by feature, keyword, or reference.

| Feature | Section | Spec | Grammar | Keywords |
|---------|---------|------|---------|----------|
| Simple sentence | [1.1](#11-simple-sentences) | 2 | SimpleSent | basic, subject, verb |
| Sentence conjunction | [1.2](#12-sentence-coordination) | 2.1 | ConjunctSent | `, and`, `, but`, `, whereas` |
| Sentence disjunction | [1.2](#12-sentence-coordination) | 2.1 | DisjunctSent | `, or`, DNF |
| Sentence either/or | [1.2](#12-sentence-coordination) | 2.1 | Sentence | `either`, `or`, exclusive |
| Sentence neither/nor | [1.2](#12-sentence-coordination) | 2.1 | Sentence | `neither`, `nor`, negative |
| Copula is/are | [2.1](#21-copula-forms) | 3.9 | VPHead | `is`, `are`, state |
| Copula negation | [2.1](#21-copula-forms) | 3.9 | VPNeg | `is not`, `are not` |
| Verb with argument | [2.2](#22-verbs-with-arguments) | 3.9 | VPHead | NP, Adj, unified |
| VP parallel conjunction | [3.1](#31-vp-operators) | 3.1 | VerbConj | `&&`, parallel, simultaneous |
| VP sequential conjunction | [3.1](#31-vp-operators) | 3.1 | VerbConj | `&& then`, sequential, ordered |
| VP inclusive disjunction | [3.1](#31-vp-operators) | 3.1 | VerbConj | `&&/`, inclusive, or |
| VP either/or | [3.2](#32-fixed-english-vp-forms) | 3.1 | EitherVP | `either`, `or`, exclusive |
| VP neither/nor | [3.2](#32-fixed-english-vp-forms) | 3.1 | NeitherVP | `neither`, `nor`, negative |
| VP precedence | [3.3](#33-vp-precedence-and-grouping) | 3.2 | VPBody | left-associative, precedence |
| Guillemets grouping | [3.3](#33-vp-precedence-and-grouping) | 3.2, 9 | GroupBody | `«»`, override, scope |
| Guillemets nested | [3.3](#33-vp-precedence-and-grouping) | 9 | GroupBody | nested, complex |
| Adverb post-verbal | [4.1](#41-adverb-placement) | 3.4 | SimpleVPCore | post-verbal, placement |
| Adverb pre-verbal | [4.1](#41-adverb-placement) | 3.4 | SimpleVPCore | pre-verbal, placement |
| Adverb coordination | [4.2](#42-adverb-coordination) | 3.4 | Adverb | `&`, coordination |
| Adverb scope narrow | [4.3](#43-adverb-scope) | 3.4 | SimpleVPCore | narrow, single-VP |
| Adverb scope wide | [4.3](#43-adverb-scope) | 3.4 | GroupVPCore | wide, guillemets |
| Modal must | [5.1](#51-deontic-modals) | 3.5 | Modal | `must`, obligation |
| Modal must not | [5.1](#51-deontic-modals) | 3.5 | Modal | `must not`, prohibition |
| Modal should | [5.1](#51-deontic-modals) | 3.5 | Modal | `should`, recommendation |
| Modal should not | [5.1](#51-deontic-modals) | 3.5 | Modal | `should not`, recommendation-against |
| Modal able to | [5.2](#52-capability-modals) | 3.5 | Modal | `is able to`, `are able to`, capability |
| Modal not able to | [5.2](#52-capability-modals) | 3.5 | Modal | `is not able to`, incapability |
| VP negation does not | [6](#6-vp-negation) | 3.6 | VPNeg | `does not`, singular |
| VP negation do not | [6](#6-vp-negation) | 3.6 | VPNeg | `do not`, plural |
| VP negation scope | [6](#6-vp-negation) | 3.6 | VPNeg | scope, guillemets |
| Contractions | [6.1](#61-contractions) | 3.6.1 | VPNeg, Modal, FutureAux | `isn't`, `aren't`, `doesn't`, `don't`, `hasn't`, `haven't`, `won't`, `mustn't`, `shouldn't`, `can't` |
| VP list parallel | [7.1](#71-parallel-conjunction-default) | 3.7 | VPList | `:`, markdown, parallel |
| VP list sequential | [7.2](#72-sequential-conjunction) | 3.7 | VPList | `then`, sequential, ordered |
| VP list exclusive | [7.3](#73-exclusive-disjunction) | 3.7 | VPList | `either:`, `or`, exclusive |
| VP list inclusive | [7.4](#74-inclusive-disjunction) | 3.7 | VPList | `or`, inclusive |
| VP list negative | [7.5](#75-negative-conjunction) | 3.7 | VPList | `neither:`, `nor`, none |
| VP list after sentence | [7.6](#76-vp-list-after-sentence-coordination) | 3.7 | VPList | sentence-coordination |
| VP list after inline | [7.7](#77-vp-list-after-inline-vp-coordination) | 3.7 | VPList | inline-coordination, `&&:` |
| NP conjunction | [8.1](#81-np-operators) | 7.1 | NounConj | `&`, both |
| NP exclusive disjunction | [8.1](#81-np-operators) | 7.1 | NounConj | `/`, one-not-both |
| NP inclusive disjunction | [8.1](#81-np-operators) | 7.1 | NounConj | `&/`, one-both-either |
| Adjective coordination | [8.2](#82-adjective-coordination) | 7.1 | AdjConj | `&`, `/`, adjective |
| Proper noun | [9.1](#91-proper-nouns) | 7.3 | ProperNP | capitalized, no-determiner |
| Unique noun | [9.2](#92-unique-nouns) | 7.3 | UniqueNP | capitalized, with-determiner |
| Mass noun | [9.3](#93-mass-nouns) | 7.3 | MassNP | uncountable, mass-determiner |
| Determiner enough | [9.4](#94-special-determiners) | 7.4 | Determiner | `enough`, plural |
| Determiner a distinct | [9.4](#94-special-determiners) | 7.4 | Determiner | `a distinct`, universal-scope |
| Determiner count | [9.4](#94-special-determiners) | 7.4 | Determiner | `at least`, `exactly`, `between` |
| Binding implicit | [10.1](#101-binding-forms) | 7.6 | Binding | juxtaposition |
| Binding explicit | [10.1](#101-binding-forms) | 7.6 | Binding | `, named`, `, called` |
| Binding identifier | [10.1](#101-binding-forms) | 7.6 | Binding | neutral-identifier |
| Pronoun resolution | [11.1](#111-pronoun-resolution) | 7.5 | PronounNP | `he`, `she`, `it`, `they` |
| Reflexive action | [11.2](#112-reflexive-actions) | 7.5 | SelfVerb | `self-` prefix |
| Reflexive possession | [11.2](#112-reflexive-actions) | 7.5 | Determiner | `his own`, `her own` |
| Relative clause basic | [12.1](#121-basic-relative-clauses) | 3.8 | RelativeClause | `that` |
| Present perfect has | [12.2](#122-present-perfect-in-relative-clauses) | 3.8 | PerfectVP | `has`, `have`, prior-event |
| Present perfect passive | [12.2](#122-present-perfect-in-relative-clauses) | 3.8 | PerfectVP | `has been`, passive |
| Present perfect copula | [12.2](#122-present-perfect-in-relative-clauses) | 3.8 | PerfectVP | `has been`, NP, Adj |
| Present perfect negation | [12.3](#123-present-perfect-negation) | 3.8 | PerfectNeg | `has not`, `has never` |
| Present perfect coordination | [12.4](#124-present-perfect-with-coordination) | 3.8 | PerfectGroupVP | guillemets, scope |
| Of-clause | [13.1](#131-of-clauses) | 8.3 | OfClause | `of`, binds-to-noun |
| Non-of PP default | [13.2](#132-non-of-prepositional-phrases) | 8.3 | NonOfPrepPhrase | binds-to-verb |
| Non-of PP guillemets | [13.2](#132-non-of-prepositional-phrases) | 8.3 | NonOfPrepPhrase | guillemets, binds-to-noun |
| Measurement phrase basic | [14.1](#141-basic-measurement-phrases) | 8.7 | MeasurementNP | Number + Unit + `of` + MassNoun |
| Measurement in comparison | [14.2](#142-measurement-phrases-in-comparisons) | 8.7 | MeasurementNP | range, comparisons |
| Mass comparison more/less | [15.1](#151-moreless-comparisons) | 8.8 | MassDeterminer | `more`/`less` + MassNP + `than` |
| Mass comparison amount | [15.2](#152-amount-of) | 8.8 | AmountOfNP | `the amount of` + MassNP |
| Partitive proportion | [16.1](#161-proportion-partitives) | 8.9 | ProportionPartitiveHead | `most of`, `%`, mass |
| Partitive count | [16.2](#162-count-partitives) | 8.9 | CountPartitiveHead | `N of the`, count |
| Exception suffix | [17](#17-exception-suffix) | 8.10 | ExceptSuffix | `except` |
| Exception pro-form | [17](#17-exception-suffix) | 8.10 | ExceptProNP | `the one(s) that` |
| Existential there is | [18](#18-existential-sentences) | 5 | ExistentialSent | `there is`, `there are` |
| Existential negation | [18](#18-existential-sentences) | 5 | ExistentialNeg | `there is not` |
| Comparative adjective | [19.1](#191-comparative-adjectives) | 6.1 | Comparison | `is` + ComparativeAdj + `than` |
| Comparative adverb | [19.2](#192-comparative-adverbs) | 6.2 | ComparativeAdv | VP + ComparativeAdv + `than` |
| Superlative adjective | [19.3](#193-superlative-adjectives) | 6.3 | Comparison | `is the` + SuperlativeAdj |
| Superlative adverb | [19.4](#194-superlative-adverbs) | 6.4 | SuperlativeAdv | VP + `the` + SuperlativeAdv |
| Equality comparison | [19.5](#195-equality-and-inequality) | 6.5 | Comparison | `is equal to`, `is not equal to` |
| Range comparison | [19.6](#196-range-comparisons) | 6.6 | Comparison | `is between` + NP |
| Conditional if | [20.1](#201-if-then-conditionals) | 7 | ConditionalSent | `if`, `then` |
| Conditional present perfect | [20.1](#201-if-then-conditionals) | 7 | ConditionBody | `has`, condition |
| Conditional exception | [20.2](#202-exception-conditionals) | 7.2 | ConditionalSent | `except when`, `except if` |
| Conditional list parallel | [20.3](#203-conditional-statement-lists-markdown) | 7.3 | ConditionalSentWithList | `:`, parallel |
| Conditional list sequential | [20.3](#203-conditional-statement-lists-markdown) | 7.3 | ConditionalSentWithList | `then`, sequential |
| Conditional list exclusive | [20.3](#203-conditional-statement-lists-markdown) | 7.3 | ConditionalSentWithList | `either:`, exclusive |
| Conditional list inclusive | [20.3](#203-conditional-statement-lists-markdown) | 7.3 | ConditionalSentWithList | `or`, inclusive |
| Whenever basic | [21](#21-whenever-universal-reactive) | 7 | CondKeyword | `whenever`, universal-reactive |
| Whenever list | [21.1](#211-whenever-lists-markdown) | 7 | ConditionalSentWithList | `whenever:`, list |
| Once basic | [22](#22-once-one-time-trigger) | 7 | CondKeyword | `once`, one-time-trigger |
| Once list | [22.1](#221-once-lists-markdown) | 7 | ConditionalSentWithList | `once:`, list |
| Each time basic | [23](#23-each-time-repeated-trigger) | 7 | CondKeyword | `each time`, repeated-trigger |
| Each time list | [23.1](#231-each-time-lists-markdown) | 7 | ConditionalSentWithList | `each time:`, list |
| Otherwise | [24](#24-otherwise-sentence) | 7.1 | OtherwiseSent | `Otherwise,` |
| Passive basic | [25.1](#251-basic-passive) | 3.10 | VerbArg | `is/are` + PastPart |
| Passive negation | [25.2](#252-passive-negation) | 3.10 | VPNeg | `is not`, passive |
| Passive agent | [25.3](#253-passive-with-agent) | 3.10 | NonOfPrepPhrase | `by`, agent |
| Passive modal | [25.4](#254-passive-with-modals) | 3.10 | Modal | `must be`, passive |
| Passive coordination | [25.5](#255-passive-with-coordination) | 3.10 | AdjConj | coordinated-complements |
| Passive PP | [25.6](#256-passive-with-prepositional-phrases) | 3.10 | NonOfPrepPhrase | PP, passive |
| String literal basic | [26.1](#261-basic-string-literals) | 8 | StringLiteral | `"..."`, `` `...` `` |
| String literal invisibility | [26.2](#262-string-literal-invisibility-pronoun-resolution) | 8.2 | StringLiteral | invisible, pronoun |
| String literal conditional | [26.3](#263-string-literals-in-conditionals) | 8 | StringLiteral | conditional |
| String literal escaping | [26.4](#264-string-literal-escaping) | 8.1 | StringLiteral | `\"`, `\\` |
| String literal argument | [26.5](#265-string-literals-as-verb-arguments) | 8 | VerbArg | argument |
| Code block after | [27.1](#271-code-block-after-sentence) | 8.4 | CodeBlock | ` ``` `, after |
| Code block before | [27.2](#272-code-block-before-sentence) | 8.4 | CodeBlock | ` ``` `, before |
| Code block multiple | [27.3](#273-multiple-code-blocks-in-sequence) | 8.4 | CodeBlock | sequence |
| Code block invisibility | [27.4](#274-code-block-with-pronoun-across-block) | 8.4 | CodeBlock | invisible, pronoun |
| Code block tilde | [27.8](#278-triple-tilde-delimiter) | 8.4 | TripleTildeBlock | `~~~` |
| Future will | [28.1](#281-basic-future-forms) | 4.1 | FutureAux | `will` |
| Future will always | [28.1](#281-basic-future-forms) | 4.1 | FutureAux | `will always` |
| Future will eventually | [28.1](#281-basic-future-forms) | 4.1 | FutureAux | `will eventually` |
| Future will immediately | [28.1](#281-basic-future-forms) | 4.1 | FutureAux | `will immediately` |
| Future will then | [28.1](#281-basic-future-forms) | 4.1 | FutureAux | `will then` |
| Future will never | [28.1](#281-basic-future-forms) | 4.1 | FutureAux | `will never` |
| Future will not | [28.1](#281-basic-future-forms) | 4.1 | FutureAux | `will not` |
| Temporal before | [28.2](#282-temporal-scopes) | 4.2 | TemporalPrefix | `before` |
| Temporal after | [28.2](#282-temporal-scopes) | 4.2 | TemporalPrefix | `after` |
| Temporal until | [28.3](#283-continuous-temporal-conditions) | 4.3 | TemporalSuffix | `until` |
| Temporal unless | [28.3](#283-continuous-temporal-conditions) | 4.3 | TemporalSuffix | `unless` |
| Temporal while | [28.3](#283-continuous-temporal-conditions) | 4.3 | TemporalPrefix | `while` |
| Temporal first/then | [28.4](#284-temporal-sequences) | 4.4 | TemporalSequence | `first`, `then`, `finally` |
| Temporal within | [28.5](#285-temporal-bounds-durations) | 4.5 | TemporalSuffix | `within`, duration |
| Temporal for | [28.5](#285-temporal-bounds-durations) | 4.5 | TemporalSuffix | `for`, duration |
| Temporal every | [28.5](#285-temporal-bounds-durations) | 4.5 | TemporalSuffix | `every`, duration |
| Temporal coordination | [28.6](#286-temporal-with-coordination) | 4.6 | FutureGroupVP | guillemets, future |
| Purpose clause prefix | [29.1](#291-prefix-form) | 10 | PurposeSent | `In order to` + VP + `,` + S |
| Purpose clause suffix | [29.2](#292-suffix-form) | 10 | PurposeSent | S + `, in order to` + VP |
| Purpose clause incidental | [29.3](#293-incidental-form) | 10 | PurposeSent | S₁ + `, in order to` + VP + `,` + S₂ |
| Purpose clause non-normative | [29.7](#297-non-normative-nature) | 10 | PurposeSent | documentary only |
| Annotation key-value | [30.1](#301-basic-annotations-key-value-pairs) | 11 | Annotation | `[[ key: value ]]` |
| Annotation multiple | [30.2](#302-multiple-annotations) | 11 | Annotation | multiple lines |
| Annotation free text | [30.3](#303-free-text-annotations) | 11 | Annotation | `[[ text ]]` |
| Annotation traceability | [30.4](#304-traceability-annotations) | 11 | Annotation | links, references |
| Annotation versioning | [30.5](#305-versioning-and-status-annotations) | 11 | Annotation | version, status |
| Annotation with code | [30.6](#306-annotations-with-code-blocks) | 11 | Annotation | precedes code block |
| Annotation non-normative | [30.10](#3010-non-normative-nature) | 11 | Annotation | documentary only |
| List inline one of | [31.1](#311-inline-lists-with-one-of) | 8.11 | InlineList | `one of:`, exclusive |
| List inline any of | [31.2](#312-inline-lists-with-any-of) | 8.11 | InlineList | `any of:`, inclusive |
| List inline all of | [31.3](#313-inline-lists-with-all-of) | 8.11 | InlineList | `all of:`, conjunction |
| List inline none of | [31.4](#314-inline-lists-with-none-of) | 8.11 | InlineList | `none of:`, exclusion |
| List inline full NPs | [31.5](#315-inline-lists-with-full-nps) | 8.11 | InlineList | full noun phrases |
| List inline mixed | [31.6](#316-inline-lists-with-mixed-types) | 8.11 | InlineList | numbers, strings, NPs |
| List guillemeted | [31.7](#317-guillemeted-inline-lists) | 8.11 | GuillemettedList | `«»`, multiple lists |
| List markdown one of | [31.9](#319-markdown-lists-with-one-of) | 8.11 | NPListSent | markdown, exclusive |
| List markdown any of | [31.10](#3110-markdown-lists-with-any-of) | 8.11 | NPListSent | markdown, inclusive |
| List markdown all of | [31.11](#3111-markdown-lists-with-all-of) | 8.11 | NPListSent | markdown, conjunction |
| List markdown none of | [31.12](#3112-markdown-lists-with-none-of) | 8.11 | NPListSent | markdown, exclusion |
| List open ellipsis | [31.15](#3115-open-lists-ellipsis) | 8.11.4 | Ellipsis | `...`, `…`, open, extensible |
| List exhaustive | [31.15](#3115-open-lists-ellipsis) | 8.11.4 | - | closed, exhaustive, default |

---

## Table of Contents

1. [Basic Sentence Structure](#1-basic-sentence-structure)
   - 1.1 [Simple Sentences](#11-simple-sentences)
   - 1.2 [Sentence Coordination](#12-sentence-coordination)
2. [Unified Verb Syntax](#2-unified-verb-syntax)
   - 2.1 [Copula Forms](#21-copula-forms)
   - 2.2 [Verbs with Arguments](#22-verbs-with-arguments)
3. [Verb Phrase (VP) Coordination](#3-verb-phrase-vp-coordination)
   - 3.1 [VP Operators](#31-vp-operators)
   - 3.2 [Fixed English VP Forms](#32-fixed-english-vp-forms)
   - 3.3 [VP Precedence and Grouping](#33-vp-precedence-and-grouping)
4. [Adverbs](#4-adverbs)
   - 4.1 [Adverb Placement](#41-adverb-placement)
   - 4.2 [Adverb Coordination](#42-adverb-coordination)
   - 4.3 [Adverb Scope](#43-adverb-scope)
5. [Modal Auxiliaries](#5-modal-auxiliaries)
   - 5.1 [Deontic Modals](#51-deontic-modals)
   - 5.2 [Capability Modals](#52-capability-modals)
6. [VP Negation](#6-vp-negation)
   - 6.1 [Contractions](#61-contractions)
7. [VP List Coordination (Markdown)](#7-vp-list-coordination-markdown)
   - 7.1 [Parallel Conjunction (Default)](#71-parallel-conjunction-default)
   - 7.2 [Sequential Conjunction](#72-sequential-conjunction)
   - 7.3 [Exclusive Disjunction](#73-exclusive-disjunction)
   - 7.4 [Inclusive Disjunction](#74-inclusive-disjunction)
   - 7.5 [Negative Conjunction](#75-negative-conjunction)
   - 7.6 [VP List After Sentence Coordination](#76-vp-list-after-sentence-coordination)
   - 7.7 [VP List After Inline VP Coordination](#77-vp-list-after-inline-vp-coordination)
8. [Noun Phrase (NP) Coordination](#8-noun-phrase-np-coordination)
   - 8.1 [NP Operators](#81-np-operators)
   - 8.2 [Adjective Coordination](#82-adjective-coordination)
9. [Noun Types](#9-noun-types)
   - 9.1 [Proper Nouns](#91-proper-nouns)
   - 9.2 [Unique Nouns](#92-unique-nouns)
   - 9.3 [Mass Nouns](#93-mass-nouns)
   - 9.4 [Special Determiners](#94-special-determiners)
10. [Binding](#10-binding)
    - 10.1 [Binding Forms](#101-binding-forms)
11. [Pronouns](#11-pronouns)
    - 11.1 [Pronoun Resolution](#111-pronoun-resolution)
    - 11.2 [Reflexive Actions](#112-reflexive-actions)
12. [Relative Clauses](#12-relative-clauses)
    - 12.1 [Basic Relative Clauses](#121-basic-relative-clauses)
    - 12.2 [Present Perfect in Relative Clauses](#122-present-perfect-in-relative-clauses)
    - 12.3 [Present Perfect Negation](#123-present-perfect-negation)
    - 12.4 [Present Perfect with Coordination](#124-present-perfect-with-coordination)
13. [Prepositional Phrases](#13-prepositional-phrases)
    - 13.1 [Of-Clauses](#131-of-clauses)
    - 13.2 [Non-Of Prepositional Phrases](#132-non-of-prepositional-phrases)
14. [Measurement Phrases](#14-measurement-phrases)
    - 14.1 [Basic Measurement Phrases](#141-basic-measurement-phrases)
    - 14.2 [Measurement Phrases in Comparisons](#142-measurement-phrases-in-comparisons)
15. [Mass Comparisons](#15-mass-comparisons)
    - 15.1 [More/Less Comparisons](#151-moreless-comparisons)
    - 15.2 [Amount Of](#152-amount-of)
16. [Partitives](#16-partitives)
    - 16.1 [Proportion Partitives](#161-proportion-partitives)
    - 16.2 [Count Partitives](#162-count-partitives)
17. [Exception Suffix](#17-exception-suffix)
18. [Existential Sentences](#18-existential-sentences)
19. [Comparisons](#19-comparisons)
    - 19.1 [Comparative Adjectives](#191-comparative-adjectives)
    - 19.2 [Comparative Adverbs](#192-comparative-adverbs)
    - 19.3 [Superlative Adjectives](#193-superlative-adjectives)
    - 19.4 [Superlative Adverbs](#194-superlative-adverbs)
    - 19.5 [Equality and Inequality](#195-equality-and-inequality)
    - 19.6 [Range Comparisons](#196-range-comparisons)
20. [Conditional Statements](#20-conditional-statements)
    - 20.1 [If-Then Conditionals](#201-if-then-conditionals)
    - 20.2 [Exception Conditionals](#202-exception-conditionals)
    - 20.3 [Conditional Statement Lists (Markdown)](#203-conditional-statement-lists-markdown)
21. [Whenever (Universal Reactive)](#21-whenever-universal-reactive)
    - 21.1 [Whenever Lists (Markdown)](#211-whenever-lists-markdown)
22. [Once (One-Time Trigger)](#22-once-one-time-trigger)
    - 22.1 [Once Lists (Markdown)](#221-once-lists-markdown)
23. [Each Time (Repeated Trigger)](#23-each-time-repeated-trigger)
    - 23.1 [Each Time Lists (Markdown)](#231-each-time-lists-markdown)
24. [Otherwise Sentence](#24-otherwise-sentence)
25. [Passive Voice](#25-passive-voice)
    - 25.1 [Basic Passive](#251-basic-passive)
    - 25.2 [Passive Negation](#252-passive-negation)
    - 25.3 [Passive with Agent](#253-passive-with-agent)
    - 25.4 [Passive with Modals](#254-passive-with-modals)
    - 25.5 [Passive with Coordination](#255-passive-with-coordination)
    - 25.6 [Passive with Prepositional Phrases](#256-passive-with-prepositional-phrases)
26. [String Literals](#26-string-literals)
    - 26.1 [Basic String Literals](#261-basic-string-literals)
    - 26.2 [String Literal Invisibility (Pronoun Resolution)](#262-string-literal-invisibility-pronoun-resolution)
    - 26.3 [String Literals in Conditionals](#263-string-literals-in-conditionals)
    - 26.4 [String Literal Escaping](#264-string-literal-escaping)
    - 26.5 [String Literals as Verb Arguments](#265-string-literals-as-verb-arguments)
27. [Fenced Code Blocks](#27-fenced-code-blocks)
    - 27.1 [Code Block After Sentence](#271-code-block-after-sentence)
    - 27.2 [Code Block Before Sentence](#272-code-block-before-sentence)
    - 27.3 [Multiple Code Blocks in Sequence](#273-multiple-code-blocks-in-sequence)
    - 27.4 [Code Block with Pronoun Across Block](#274-code-block-with-pronoun-across-block)
    - 27.5 [Data Format Specification](#275-data-format-specification)
    - 27.6 [Algorithm Specification](#276-algorithm-specification)
    - 27.7 [SQL Query Example](#277-sql-query-example)
    - 27.8 [Triple Tilde Delimiter](#278-triple-tilde-delimiter)
28. [Temporal and Future Statements](#28-temporal-and-future-statements)
    - 28.1 [Basic Future Forms](#281-basic-future-forms)
    - 28.2 [Temporal Scopes](#282-temporal-scopes)
    - 28.3 [Continuous Temporal Conditions](#283-continuous-temporal-conditions)
    - 28.4 [Temporal Sequences](#284-temporal-sequences)
    - 28.5 [Temporal Bounds (Durations)](#285-temporal-bounds-durations)
    - 28.6 [Temporal with Coordination](#286-temporal-with-coordination)
29. [Purpose Clauses](#29-purpose-clauses)
    - 29.1 [Prefix Form](#291-prefix-form)
    - 29.2 [Suffix Form](#292-suffix-form)
    - 29.3 [Incidental Form](#293-incidental-form)
    - 29.4 [Purpose Clauses with Modals](#294-purpose-clauses-with-modals)
    - 29.5 [Purpose Clauses with Conditionals](#295-purpose-clauses-with-conditionals)
    - 29.6 [Complex Purpose VPs](#296-complex-purpose-vps)
    - 29.7 [Non-Normative Nature](#297-non-normative-nature)
30. [Annotations](#30-annotations)
    - 30.1 [Basic Annotations (Key-Value Pairs)](#301-basic-annotations-key-value-pairs)
    - 30.2 [Multiple Annotations](#302-multiple-annotations)
    - 30.3 [Free Text Annotations](#303-free-text-annotations)
    - 30.4 [Traceability Annotations](#304-traceability-annotations)
    - 30.5 [Versioning and Status Annotations](#305-versioning-and-status-annotations)
    - 30.6 [Annotations with Code Blocks](#306-annotations-with-code-blocks)
    - 30.7 [Annotations with Conditionals](#307-annotations-with-conditionals)
    - 30.8 [Annotations with Temporal Statements](#308-annotations-with-temporal-statements)
    - 30.9 [Mixed Annotations](#309-mixed-annotations)
    - 30.10 [Non-Normative Nature](#3010-non-normative-nature)
31. [Lists/Enumeration](#31-listsenumeration)
    - 31.1 [Inline Lists with `one of:`](#311-inline-lists-with-one-of)
    - 31.2 [Inline Lists with `any of:`](#312-inline-lists-with-any-of)
    - 31.3 [Inline Lists with `all of:`](#313-inline-lists-with-all-of)
    - 31.4 [Inline Lists with `none of:`](#314-inline-lists-with-none-of)
    - 31.5 [Inline Lists with Full NPs](#315-inline-lists-with-full-nps)
    - 31.6 [Inline Lists with Mixed Types](#316-inline-lists-with-mixed-types)
    - 31.7 [Guillemeted Inline Lists](#317-guillemeted-inline-lists)
    - 31.8 [Guillemeted Lists Mid-Sentence](#318-guillemeted-lists-mid-sentence)
    - 31.9 [Markdown Lists with `one of:`](#319-markdown-lists-with-one-of)
    - 31.10 [Markdown Lists with `any of:`](#3110-markdown-lists-with-any-of)
    - 31.11 [Markdown Lists with `all of:`](#3111-markdown-lists-with-all-of)
    - 31.12 [Markdown Lists with `none of:`](#3112-markdown-lists-with-none-of)
    - 31.13 [Markdown Lists with Full NPs](#3113-markdown-lists-with-full-nps)
    - 31.14 [Single-Item Lists](#3114-single-item-lists)
    - 31.15 [Open Lists (Ellipsis)](#3115-open-lists-ellipsis)

---

## 1. Basic Sentence Structure

### 1.1 Simple Sentences

```sloj
"A user logs-in."
```
*Simple sentence with subject and verb*

### 1.2 Sentence Coordination

```sloj
"A user logs-in, and the system verifies credentials."
```
*Sentence conjunction with `, and`*

```sloj
"The system is active, but the alarm-light is red."
```
*Sentence conjunction with `, but`*

```sloj
"The system is active, whereas the alarm-light is red."
```
*Sentence conjunction with `, whereas`*

```sloj
"A user logs-in, and the system verifies credentials, or the connection times-out."
```
*Disjunctive Normal Form (DNF): disjunction of conjunctions*

```sloj
"Either a user logs-in, or the connection times-out."
```
*Sentence-level exclusive disjunction*

```sloj
"Neither a user logs-in, nor the connection times-out."
```
*Sentence-level negative conjunction*

---

## 2. Unified Verb Syntax

### 2.1 Copula Forms

```sloj
"A dog is fast."
"The dogs are fast."
```
*Copula: NP is/are Adj*

```sloj
"The alarm-light is green."
"The alarm-lights are not green."
```
*Copula with state/classification and negation*

```sloj
"A dog is in the park."
"The dog is not in the park."
```
*Copula: NP is/are PP (locative)*

```sloj
"The alarm-light must be green."
```
*Infinitive copula with modal*

### 2.2 Verbs with Arguments

```sloj
"A dog runs."
```
*Verb with no argument*

```sloj
"John runs a company."
```
*Verb with NP argument*

```sloj
"The light becomes green."
```
*Verb with Adj argument*

```sloj
"The light becomes a beacon."
```
*Verb with NP argument*

---

## 3. Verb Phrase (VP) Coordination

### 3.1 VP Operators

```sloj
"The system validates the input && logs the result."
```
*Parallel conjunction (`&&`): simultaneous/unordered*

```sloj
"The system validates the input && then logs the result."
```
*Sequential conjunction (`&& then`): ordered*

```sloj
"The system validates the input &&/ logs the error."
```
*Inclusive disjunction (`&&/`): one or both*

### 3.2 Fixed English VP Forms

```sloj
"The system either validates the input, or logs the error."
```
*Exclusive disjunction: `either VP, or VP`*

```sloj
"The system neither validates the input, nor logs the error."
```
*Negative conjunction: `neither VP, nor VP`*

### 3.3 VP Precedence and Grouping

```sloj
"The system validates the input &&/ logs the error && then sends a notification."
```
*Left-associative: `((validates &&/ logs) && then sends)`*

```sloj
"The system validates the input &&/ «logs the error && then sends a notification»."
```
*Guillemets override precedence: `(validates &&/ (logs && then sends))`*

```sloj
"The system ««validates the input && logs the result» &&/ sends a notification» && then completes."
```
*Nested guillemet grouping*

---

## 4. Adverbs

### 4.1 Adverb Placement

```sloj
"A dog barks loudly."
```
*Post-verbal adverb*

```sloj
"A dog loudly barks."
```
*Pre-verbal adverb*

### 4.2 Adverb Coordination

```sloj
"A user quickly & silently logs-in."
```
*Adverb phrase coordination with `&`*

### 4.3 Adverb Scope

```sloj
"A boy quickly runs && talks."
```
*Adverb modifies first VP only*

```sloj
"A boy quickly «runs && talks»."
```
*Adverb modifies entire guillemeted group*

---

## 5. Modal Auxiliaries

### 5.1 Deontic Modals

```sloj
"The system must validate the input && log the result."
```
*Modal with guillemeted scope over coordinated VPs*

```sloj
"The system must not validate the input."
```
*Prohibition: `must not`*

```sloj
"The system should not log debug events."
```
*Recommendation against: `should not`*

### 5.2 Capability Modals

```sloj
"The servers are able to respond."
```
*Capability: `are able to`*

```sloj
"The server is not able to respond."
```
*Incapability: `is not able to`*

---

## 6. VP Negation

```sloj
"The server does not respond."
```
*VP negation: `does not` (singular)*

```sloj
"The servers do not respond."
```
*VP negation: `do not` (plural)*

```sloj
"The system does not validate the input && log the result."
```
*Negation scopes over coordinated VPs*

### 6.1 Contractions

Sloj permits contracted forms as alternatives to full forms.

```sloj
"The server doesn't respond."
```
*Contracted form: `doesn't` = `does not`*

```sloj
"The servers don't respond."
```
*Contracted form: `don't` = `do not`*

```sloj
"The alarm-lights aren't green."
```
*Contracted copula negation: `aren't` = `are not`*

```sloj
"The dog isn't in the park."
```
*Contracted copula negation: `isn't` = `is not`*

```sloj
"The system mustn't store passwords."
```
*Contracted modal: `mustn't` = `must not`*

```sloj
"The system shouldn't log debug events."
```
*Contracted modal: `shouldn't` = `should not`*

```sloj
"The system won't respond."
```
*Contracted future negation: `won't` = `will not`*

```sloj
"The user can't export data."
```
*Contracted capability negation: `can't` = `is not able to`*

```sloj
"A user that hasn't logged-in is marked as inactive."
```
*Contracted present perfect: `hasn't` = `has not`*

```sloj
"The accounts that haven't been verified are suspended."
```
*Contracted present perfect: `haven't` = `have not`*

---

## 7. VP List Coordination (Markdown)

### 7.1 Parallel Conjunction (Default)

```sloj
The system:
  - validates the input.
  - logs the result.
  - sends a notification.
```
*All VPs hold (unordered)*

### 7.2 Sequential Conjunction

```sloj
The player:
  - moves three steps.
  - then pushes the button.
  - then opens the bottle.
```
*VPs occur in order*

### 7.3 Exclusive Disjunction

```sloj
The red light either:
  - blinks.
  - or turns off.
  - or becomes green.
```
*Exactly one VP holds*

### 7.4 Inclusive Disjunction

```sloj
The system:
  - logs the error.
  - or sends an alert.
  - or retries the operation.
```
*At least one VP holds*

### 7.5 Negative Conjunction

```sloj
The cat neither:
  - purrs at strangers.
  - nor jumps on the bed.
  - nor eats fish.
```
*None of the VPs hold*

### 7.6 VP List After Sentence Coordination

```sloj
An operator pulls the lever, and the red light either:
  - blinks.
  - or turns off.
  - or becomes green.
```
*VP list follows sentence conjunction*

### 7.7 VP List After Inline VP Coordination

```sloj
A dog barks &&:
  - runs away.
  - then hides.
  - then falls asleep.
```
*VP list after `&&` with sequential markers*

```sloj
The system validates the input && either:
  - logs the result.
  - or sends a notification.
```
*VP list after `&&` with exclusive disjunction*

---

## 8. Noun Phrase (NP) Coordination

### 8.1 NP Operators

```sloj
"The system notifies the admin & the auditor."
```
*Conjunction (`&`): both entities*

```sloj
"The system notifies the admin / the guest."
```
*Exclusive disjunction (`/`): one or the other, not both*

```sloj
"The system notifies the admin &/ the guest."
```
*Inclusive disjunction (`&/`): one, both, or either*

### 8.2 Adjective Coordination

```sloj
"A user is fast & reliable."
```
*Adjective conjunction*

```sloj
"A user is fast / reliable."
```
*Adjective exclusive disjunction*

---

## 9. Noun Types

### 9.1 Proper Nouns

```sloj
"John is a user."
```
*Proper noun (no determiner)*

### 9.2 Unique Nouns

```sloj
"The GPS is active."
```
*Unique noun (with determiner)*

### 9.3 Mass Nouns

```sloj
"Some water is available."
"No water is available."
"More water is available."
"Less water is available."
```
*Mass nouns with mass determiners*

### 9.4 Special Determiners

```sloj
"Enough cards remain."
```
*`enough` with plural common noun*

```sloj
"Every user has a distinct account."
```
*`a distinct` under universal quantifier scope*

```sloj
"At least 3 servers respond."
"Exactly 2 administrators respond."
"Between 3 and 5 alarms are active."
```
*Count determiners*

---

## 10. Binding

### 10.1 Binding Forms

```sloj
"A user Alice logs-in."
```
*Implicit binding (juxtaposition)*

```sloj
"A user, Bob, logs-in."
```
*Explicit binding (appositive with commas)*

```sloj
"A user, named Carol, logs-in."
```
*Explicit binding with `named`*

```sloj
"The server S1 receives a request."
```
*Binding with neutral identifier*

---

## 11. Pronouns

### 11.1 Pronoun Resolution

```sloj
"A user Bob sends a message to a user Carol. She receives it from him."
```
*Pronoun resolution: `She` → Carol, `him` → Bob, `it` → message*

```sloj
"The server S1 receives a request from the client C. S1 processes the request."
```
*Neutral identifiers: use explicit name for clarity*

### 11.2 Reflexive Actions

```sloj
"Bob self-authenticates."
"Alice self-logs-out."
```
*Reflexive action with `self-` prefix*

```sloj
"A user Bob updates his own profile."
```
*Reflexive possession: `his own`*

---

## 12. Relative Clauses

### 12.1 Basic Relative Clauses

```sloj
"A user that logs-in is active."
```
*Relative clause with VP*

```sloj
"A user that is active logs-in."
```
*Relative clause with copula VP*

### 12.2 Present Perfect in Relative Clauses

```sloj
"A customer that has logged-in receives a welcome message."
```
*Present perfect: `has` + past participle*

```sloj
"A request that has been validated is processed."
```
*Present perfect passive: `has been` + past participle*

```sloj
"A user that has been an administrator receives special access."
```
*Present perfect copula: `has been` + NP*

```sloj
"A session that has been active is renewed."
```
*Present perfect copula: `has been` + Adj*

### 12.3 Present Perfect Negation

```sloj
"A user that has never received a notification is marked as inactive."
```
*Present perfect with `has never`*

```sloj
"A user that has not logged-in is marked as inactive."
```
*Present perfect with `has not`*

```sloj
"Requests that have failed are retried."
```
*Present perfect with plural subject: `have`*

```sloj
"Users that have never been administrators are restricted."
```
*Present perfect negation with plural: `have never been`*

### 12.4 Present Perfect with Coordination

```sloj
"A request that «has failed && been retried» is escalated."
```
*Guillemets scope present perfect over coordinated VPs*

```sloj
"John sends the message that has been prepared by Mark."
```
*Present perfect passive with by-phrase*

---

## 13. Prepositional Phrases

### 13.1 Of-Clauses

```sloj
"The size of the file is large."
```
*Of-clause always binds to preceding noun*

### 13.2 Non-Of Prepositional Phrases

```sloj
"A user sends a message to the server."
```
*Non-of PP attaches to verb by default*

```sloj
"A user sends «a message to the server»."
```
*Guillemets attach non-of PP to object NP*

---

## 14. Measurement Phrases

### 14.1 Basic Measurement Phrases

```sloj
"3 liters of water are added."
```
*Measurement phrase with plural agreement*

```sloj
"1 liter of water is added."
```
*Measurement phrase with singular agreement (Number = 1)*

```sloj
"500 megabytes of data are cached."
```
*Measurement phrase with digital storage unit*

```sloj
"The timeout is 30 seconds."
```
*Measurement phrase as predicate*

### 14.2 Measurement Phrases in Comparisons

```sloj
"The temperature is between 0 degrees & 100 degrees."
```
*Range comparison with measurement phrases*

```sloj
"The response-time is between 30 seconds & 40 seconds."
```
*Range with time measurements*

```sloj
"The tank contains more than 50 liters of water."
```
*Comparative with measurement phrase*

---

## 15. Mass Comparisons

### 15.1 More/Less Comparisons

```sloj
"The tank contains more water than fuel."
```
*Mass comparison: `more` MassNP `than` MassNP*

```sloj
"The process uses less memory than bandwidth."
```
*Mass comparison: `less` MassNP `than` MassNP*

### 15.2 Amount Of

```sloj
"The amount of data is large."
```
*`the amount of` converts mass noun to countable*

```sloj
"The amount of water is greater than 5 liters."
```
*`the amount of` with comparison*

---

## 16. Partitives

### 16.1 Proportion Partitives

```sloj
"Most of the data is backed up."
```
*Proportion partitive with mass noun*

```sloj
"30% of the data is corrupted."
```
*Percent partitive*

### 16.2 Count Partitives

```sloj
"Between 3 and 5 of the LEDs blink."
```
*Count partitive with range*

---

## 17. Exception Suffix

```sloj
"The dealer gives all of the cards except the cards that show a red mark."
```
*Exception suffix: `except` + NP*

```sloj
"The dealer gives all of the cards except the ones that show a red mark."
```
*Exception pro-form: `except the one(s) that` VP*

---

## 18. Existential Sentences

```sloj
"There is a dog."
"There are three cats."
```
*Existential: `there is/are` + NP*

```sloj
"There is not a valid token."
```
*Existential negation*

```sloj
"There are no users in the system."
```
*Existential with `no` + locative PP*

---

## 19. Comparisons

### 19.1 Comparative Adjectives

```sloj
"Tank A is larger than Tank B."
```
*Comparative adjective with NP comparand*

```sloj
"The temperature is greater than 100."
```
*Comparative adjective with number comparand*

```sloj
"The count is less than 5."
```
*Comparative: `less than`*

### 19.2 Comparative Adverbs

```sloj
"Process A completes more-quickly than Process B."
```
*Comparative adverb with NP comparand*

```sloj
"The server responds faster than the client requests."
```
*Comparative adverb with VP comparand*

### 19.3 Superlative Adjectives

```sloj
"Tank A is the largest."
```
*Superlative adjective without domain*

```sloj
"Tank A is the largest of the three tanks."
```
*Superlative adjective with `of` domain*

### 19.4 Superlative Adverbs

```sloj
"Process A completes the most-quickly."
```
*Superlative adverb without domain*

```sloj
"Process A completes the fastest of all processes."
```
*Superlative adverb with `of` domain*

### 19.5 Equality and Inequality

```sloj
"The priority is equal to 3."
```
*Equality with number*

```sloj
"The status is equal to the previous-status."
```
*Equality with NP*

```sloj
"The count is not equal to 0."
```
*Inequality with number*

```sloj
"The result is not equal to the expected-value."
```
*Inequality with NP*

```sloj
"The status is not equal to \"pending\"."
```
*Inequality with string literal*

### 19.6 Range Comparisons

```sloj
"The level is between the limits."
```
*Range comparison with single NP*

```sloj
"The level is between the min-limit & the max-limit."
```
*Range comparison with coordinated NPs*

```sloj
"The temperature is between «0 & 100» degree-celsius."
```
*Range with coordinated numbers and unit (requires measurement phrases)*

**Note:** Bare numbers not allowed; must use noun phrases or measurement phrases.

---

## 20. Conditional Statements

### 20.1 If-Then Conditionals

```sloj
"If the card is valid, then the system accepts the card."
```
*Basic conditional with present tense*

```sloj
"If a user is logged-in, then the system displays the dashboard."
```
*Conditional with copula*

```sloj
"If a session has expired, then the user must re-authenticate."
```
*Conditional with present perfect*

```sloj
"If the payment has been confirmed, then the order is dispatched."
```
*Conditional with perfect passive*

```sloj
"If a request has not been validated, then the system rejects the request."
```
*Conditional with negated present perfect*

```sloj
"If the user has never logged-in, then the account is marked as inactive."
```
*Conditional with `has never`*

```sloj
"If the message has been prepared by Mark, then John sends the message."
```
*Conditional with perfect passive and by-phrase*

### 20.2 Exception Conditionals

```sloj
"The system logs every access, except when the system is in recovery mode."
```
*Exception with `except when` + present tense*

```sloj
"A user may access the file, except if the file is locked."
```
*Exception with `except if` + present tense*

```sloj
"The order is dispatched, except when the payment has not been confirmed."
```
*Exception with `except when` + negated perfect*

```sloj
"The account remains active, except if the user has never logged-in."
```
*Exception with `except if` + `has never`*

### 20.3 Conditional Statement Lists (Markdown)

#### Parallel Conjunction

```sloj
if a user has logged-in:
  - The system validates the session.
  - The dashboard is displayed.
  - A welcome message is sent.
```
*All statements hold*

#### Sequential Conjunction

```sloj
if the payment has been confirmed:
  - The order is dispatched.
  - then The customer receives a notification.
  - then The inventory is updated.
```
*Statements occur in order*

#### Exclusive Disjunction

```sloj
if the validation fails, either:
  - The request is rejected.
  - or The request is queued for retry.
  - or The administrator is notified.
```
*Exactly one statement holds*

#### Inclusive Disjunction

```sloj
if an error occurs:
  - The error is logged.
  - or The administrator is notified.
  - or The system retries the operation.
```
*At least one statement holds*

---

## 21. Whenever (Universal Reactive)

```sloj
"Whenever a request arrives, then the system logs it."
```
*Basic whenever form*

```sloj
"Whenever the temperature exceeds 100 degrees, then the alarm activates."
```
*State-based trigger*

```sloj
"Whenever a user has logged-in, then a session is created."
```
*Whenever with present perfect*

```sloj
"Whenever a file has been modified, then the backup system copies the file."
```
*Whenever with perfect passive*

### 21.1 Whenever Lists (Markdown)

```sloj
whenever a request arrives:
  - The system logs the request.
  - The timestamp is recorded.
  - The request-queue is updated.
```
*Parallel conjunction*

```sloj
whenever a payment has been confirmed:
  - The order is dispatched.
  - then The customer receives a confirmation.
  - then The inventory is updated.
```
*Sequential conjunction*

---

## 22. Once (One-Time Trigger)

```sloj
"Once the client is authorized, then it may access the resource."
```
*State transition with persistent effect*

```sloj
"Once the system has started, then the health monitor runs."
```
*Once with present perfect*

```sloj
"Once the payment has been confirmed, then the order is dispatched."
```
*Once with persistent effect*

```sloj
"Once the switch is triggered, then the light remains on."
```
*Simple trigger*

### 22.1 Once Lists (Markdown)

```sloj
once the system has started:
  - The health monitor runs.
  - The cache is warmed.
  - The service is registered.
```
*Parallel conjunction with persistent effects*

```sloj
once the validation fails, either:
  - The request is rejected.
  - or The request is queued for manual review.
  - or The error is logged.
```
*Exclusive disjunction*

---

## 23. Each Time (Repeated Trigger)

```sloj
"Each time the lever is pulled, then the counter increments."
```
*Discrete event trigger*

```sloj
"Each time a user logs-in, then the system records the timestamp."
```
*User action trigger*

```sloj
"Each time a transaction has completed, then a receipt is generated."
```
*Each time with present perfect*

```sloj
"Each time the button is pressed, then the LED blinks."
```
*Repeated action*

### 23.1 Each Time Lists (Markdown)

```sloj
each time a user logs-in:
  - The session is created.
  - or The existing session is renewed.
  - or The user is prompted to confirm.
```
*Inclusive disjunction*

```sloj
each time the lever is pulled:
  - The counter increments.
  - then The display is updated.
  - then A beep sounds.
```
*Sequential conjunction*

---

## 24. Otherwise Sentence

```sloj
"If a card is valid, then the system accepts it. Otherwise, the system rejects the card."
```
*Otherwise binds to preceding `if`*

```sloj
"If the user has logged-in, then the dashboard is displayed. Otherwise, the login page is displayed."
```
*Otherwise with perfect condition*

```sloj
"If the file exists, then the system reads the file. Otherwise, the system creates a new file."
```
*Otherwise with present tense*

```sloj
"Whenever a request is valid, then the system processes it. Otherwise, the system logs an error."
```
*Otherwise binds to `whenever`*

---

## 25. Passive Voice

### 25.1 Basic Passive

```sloj
"The card is inserted."
"The cards are inserted."
"The data is validated."
"The files are processed."
```
*Basic passive forms*

### 25.2 Passive Negation

```sloj
"The card is not inserted."
"The request is not approved."
"The files are not deleted."
```
*Passive with negation*

### 25.3 Passive with Agent

```sloj
"The card is inserted by the user."
"The message is prepared by Mark."
"The order is dispatched by the system."
```
*Passive with by-phrase (agent)*

### 25.4 Passive with Modals

```sloj
"The card must be validated."
"The request may be rejected."
"The data should be archived."
"The file must not be deleted."
```
*Passive with modal auxiliaries*

### 25.5 Passive with Coordination

```sloj
"The card is «validated & logged»."
"The request is «received & processed»."
"The data is «archived &/ deleted»."
```
*Passive with coordinated complements*

### 25.6 Passive with Prepositional Phrases

```sloj
"The card is inserted into the slot."
"The file is stored in the archive."
"The message is sent to the admin."
```
*Passive with PPs*

```sloj
"The card must be «validated & archived»."
```
*Passive with modal and coordinated complements*

```sloj
"The system is able to be accessed."
```
*Passive with capability modal*

---

## 26. String Literals

### 26.1 Basic String Literals

```sloj
"The status is \"active\"."
```
*String literal with copula*

```sloj
"The file is named \"config.json\"."
```
*String literal in object position*

```sloj
"The function `f(x)` is continuous."
```
*Backtick notation*

```sloj
"The query `SELECT * FROM users` is executed."
```
*SQL query in backticks*

### 26.2 String Literal Invisibility (Pronoun Resolution)

```sloj
"The function `f(x)` is monotone. It increases continuously."
```
*`It` → function (string is invisible)*

```sloj
"The file \"config.json\" is valid. It contains the settings."
```
*`It` → file (string is invisible)*

```sloj
"A user Alice sets the status to \"active\". She receives a notification."
```
*`She` → Alice (string is invisible)*

```sloj
"The server \"prod-1\" is active. It handles requests."
```
*`It` → server (string is invisible)*

```sloj
"A variable `x` is positive. It must be non-zero."
```
*`It` → variable (string is invisible)*

### 26.3 String Literals in Conditionals

```sloj
"If the status is \"active\", then the system proceeds."
"If the mode is \"debug\", then the system logs verbose output."
"Whenever the status becomes \"failed\", then the system sends an alert."
```
*String literals in conditional contexts*

### 26.4 String Literal Escaping

```sloj
"The message \"User \\\"admin\\\" logged in\" is displayed."
```
*Escaped double quotes*

```sloj
"The path \"C:\\\\Windows\\\\System32\" is valid."
```
*Escaped backslashes*

### 26.5 String Literals as Verb Arguments

```sloj
"The system sets the status to \"pending\"."
"The function returns \"success\"."
```
*String literals as arguments*

---

## 27. Fenced Code Blocks

### 27.1 Code Block After Sentence

```sloj
The algorithm processes the input.
```python
def process(x):
    return x * 2
```
It returns the doubled value.
```
*`It` → algorithm (code block is invisible)*

### 27.2 Code Block Before Sentence

```sloj
```json
{"status": "active", "count": 42}
```
The configuration has this structure. The system reads it on startup.
```
*`it` → configuration (code block is invisible)*

### 27.3 Multiple Code Blocks in Sequence

```sloj
The system implements three functions.
```python
def validate(x): pass
```
```python
def transform(x): pass
```
```python
def output(x): pass
```
Each function must be pure.
```
*`Each` → functions (code blocks are invisible)*

### 27.4 Code Block with Pronoun Across Block

```sloj
The parser processes the input.
```python
def parse(input):
    tokens = tokenize(input)
    return build_ast(tokens)
```
It constructs an abstract syntax tree.
```
*`It` → parser (code block is invisible)*

### 27.5 Data Format Specification

```sloj
The configuration file has the following structure.
```json
{
  "server": "localhost",
  "port": 8080,
  "timeout": 30
}
```
It must be valid JSON. The timeout must be positive.
```
*`It` → file (code block is invisible)*

### 27.6 Algorithm Specification

```sloj
The system implements the following sorting algorithm.
```python
def quicksort(arr):
    if len(arr) <= 1:
        return arr
    pivot = arr[0]
    return quicksort([x for x in arr[1:] if x < pivot]) + [pivot] + quicksort([x for x in arr[1:] if x >= pivot])
```
The algorithm must complete in O(n log n) time.
```
*Code block provides implementation details*

### 27.7 SQL Query Example

```sloj
The system executes this query.
```sql
SELECT u.name, u.email FROM users u WHERE u.status = 'active' ORDER BY u.created_at DESC
```
It must return at most 100 rows.
```
*`It` → query (code block is invisible)*

### 27.8 Triple Tilde Delimiter

```sloj
The script contains the following code.
~~~bash
#!/bin/bash
echo "Hello, World!"
~~~
It must be executable.
```
*`It` → script (alternative delimiter)*

---

## 28. Temporal and Future Statements

### 28.1 Basic Future Forms

```sloj
"The system will respond."
"The system will always be available."
"Every request will eventually be processed."
"The counter will immediately be incremented."
"The system will then validate the input."
"The system will never be in an error-state."
"The system will not respond."
```
*Basic future auxiliaries*

### 28.2 Temporal Scopes

```sloj
"Before initialization, the system will wait."
"After initialization, the system will always be operational."
"After startup and before shutdown, the system will log all events."
"Before the session expires, the system will send a warning."
"After the user has logged-in, the dashboard will be displayed."
```
*Temporal scope with `before`/`after`*

### 28.3 Continuous Temporal Conditions

```sloj
"The wheel will turn until the lever is engaged."
"The server will run unless a shutdown-command is received."
"While the system is active, it will monitor processes."
"The system will retry until the request succeeds."
"The process will continue unless an error occurs."
"While the connection is open, the client will send heartbeats."
```
*Temporal conditions with `until`/`unless`/`while`*

### 28.4 Temporal Sequences

```sloj
"First a customer enters a card, then the system verifies it."
"First the card is inserted, then the PIN is entered, finally the transaction is processed."
"First the user logs-in, then the session is created."
"First the data is validated, then it is stored, finally a confirmation is sent."
```
*Temporal sequences with `first`/`then`/`finally`*

### 28.5 Temporal Bounds (Durations)

```sloj
"The system will respond within 30 seconds."
"The session will remain active for 30 minutes."
"The heartbeat is sent every 30 seconds."
"The heartbeat is sent at least every 30 seconds."
"The sensor reports at most every 5 minutes."
"The sensor reports every 1 to 3 minutes."
"The system will respond within 100 milliseconds."
"The lock will remain engaged for 5 minutes."
"The backup runs every 24 hours."
"The status is checked at least every 10 seconds."
"The report is generated at most every 1 week."
"The sync occurs every 30 to 60 seconds."
```
*Duration-based temporal statements*

### 28.6 Temporal with Coordination

```sloj
"The system will «validate the input && log the result»."
"The system will always «monitor && report» errors."
"The alarm will «activate && send a notification»."
```
*Future auxiliaries with guillemeted VP coordination*

---

## 29. Purpose Clauses

**Spec Reference**: Section 10  
**Grammar Reference**: `PurposeSent`, `PurposeVP`

Purpose clauses express the **intent, rationale, or purpose** behind a requirement. They are **non-normative** and do not change the formal logic mapping of the sentence.

### 29.1 Prefix Form

```sloj
"In order to comply with regulations, the lever must be locked."
"In order to ensure data integrity, the system must validate all inputs."
"In order to protect user privacy, the application must encrypt all data."
"In order to prevent unauthorized access, the door must remain closed."
```
*Purpose clause before the main sentence*

### 29.2 Suffix Form

```sloj
"The system must log all operations, in order to ensure auditability."
"The application must encrypt all data, in order to protect user privacy."
"The door must remain closed, in order to prevent unauthorized access."
"The system must validate all inputs, in order to ensure data integrity."
```
*Purpose clause after the main sentence*

### 29.3 Incidental Form

```sloj
"The system must log all operations, in order to ensure auditability, and it must store the logs for 90 days."
"The application must encrypt data, in order to protect privacy, and it must use AES-256."
"The door must remain closed, in order to prevent access, but the alarm must be active."
```
*Purpose clause between two coordinated sentences*

### 29.4 Purpose Clauses with Modals

```sloj
"In order to maintain performance, the cache must be cleared every hour."
"The system must not log passwords, in order to protect user credentials."
"In order to ensure reliability, the backup must run daily."
"The connection must be encrypted, in order to prevent eavesdropping."
```
*Purpose clauses with deontic modals*

### 29.5 Purpose Clauses with Conditionals

```sloj
"In order to prevent data loss, if the connection fails then the system must retry."
"If the user is authenticated, in order to track activity, the system must log the session."
"Whenever a transaction occurs, in order to ensure compliance, the system must record it."
```
*Purpose clauses combined with conditional statements*

### 29.6 Complex Purpose VPs

```sloj
"The system must validate inputs, in order to prevent injection attacks."
"In order to improve response time, the server must cache frequent queries."
"The application must hash passwords, in order to protect against breaches."
"In order to enable recovery, the system must maintain transaction logs."
```
*Purpose clauses with various verb types*

### 29.7 Non-Normative Nature

**Important**: Purpose clauses are documentary only. They explain **why** a requirement exists, not **what** the requirement is.

```sloj
"The system must encrypt all data, in order to protect user privacy."
```

**Formal requirement**: "The system must encrypt all data"  
**Purpose (documentary)**: "to protect user privacy"

The purpose clause does NOT create an additional requirement to "protect user privacy" — it only explains the rationale for the encryption requirement.

---

## 30. Annotations

Annotations provide metadata for requirements without affecting their formal semantics. They use the `[[ ... ]]` syntax.

### 30.1 Basic Annotations (Key-Value Pairs)

```sloj
[[ id: REQ-001 ]]
The system must respond within 100 milliseconds.

[[ id: REQ-002, priority: high ]]
The user can submit & save the form.

[[ id: REQ-003, priority: critical, author: alice ]]
The system must encrypt all data.
```
*Annotations with key-value pairs for requirements management*

### 30.2 Multiple Annotations

```sloj
[[ id: REQ-004 ]]
[[ priority: high ]]
[[ author: alice, date: 2024-01-15 ]]
The system must validate all inputs.
```
*Multiple annotations on separate lines*

### 30.3 Free Text Annotations

```sloj
[[ This requirement addresses the performance issue reported in Q3. ]]
The cache must be cleared every 5 minutes.

[[ Based on customer feedback from the beta release. ]]
The UI must display a progress indicator.
```
*Annotations with free text for rationale or context*

### 30.4 Traceability Annotations

```sloj
[[ links: JIRA-123, TEST-456 ]]
The API must support OAuth 2.0 authentication.

[[ test-case: TC-789, design-doc: DD-042 ]]
The system must log all failed login attempts.

[[ parent: REQ-100, children: REQ-101, REQ-102 ]]
The authentication module must be secure.
```
*Annotations for linking to external systems and documents*

### 30.5 Versioning and Status Annotations

```sloj
[[ version: 2.1, date: 2024-01-15, status: approved ]]
The API must support OAuth 2.0 authentication.

[[ status: draft, reviewer: bob ]]
The system should cache frequently accessed data.

[[ deprecated: true, replaced-by: REQ-500 ]]
The system must use MD5 hashing.
```
*Annotations for version control and approval workflow*

### 30.6 Annotations with Code Blocks

```sloj
[[ id: REQ-010, format: json ]]
The API must accept requests in the following format:

```json
{
  "user": "alice",
  "action": "login"
}
```

[[ id: REQ-011, language: python ]]
The validation function must implement the following logic:

```python
def validate(input):
    return len(input) > 0 and input.isalnum()
```
```
*Annotations can precede code blocks*

### 30.7 Annotations with Conditionals

```sloj
[[ id: REQ-020, priority: high ]]
If the user is authenticated, then the system must log the session.

[[ id: REQ-021, category: security ]]
Whenever a transaction occurs, then the system must record it.
```
*Annotations with conditional statements*

### 30.8 Annotations with Temporal Statements

```sloj
[[ id: REQ-030, timing: critical ]]
The system will respond within 50 milliseconds.

[[ id: REQ-031, schedule: periodic ]]
The backup will run every 24 hours.
```
*Annotations with future and temporal statements*

### 30.9 Mixed Annotations

```sloj
[[ id: REQ-040 ]]
[[ Customer requirement from contract clause 5.2.3 ]]
[[ priority: critical, compliance: ISO-27001 ]]
The system must encrypt all data at rest & in transit.
```
*Combining key-value pairs and free text annotations*

### 30.10 Non-Normative Nature

**Important**: Annotations are documentary only. They provide metadata but do NOT affect the formal semantics of requirements.

```sloj
[[ id: REQ-050, priority: high ]]
The system must validate all inputs.
```

**Formal requirement**: "The system must validate all inputs"  
**Metadata (documentary)**: `id: REQ-050, priority: high`

The annotation does NOT change what the requirement means — it only provides metadata for requirements management tools.

---

## 31. Lists/Enumeration

Lists provide a readable way to specify multiple values or items. Three keywords determine the coordination semantics.

### 31.1 Inline Lists with `one of:`

```sloj
The value is one of: 3, "pippo", xld.
The status is one of: "active", "pending", "closed".
The color is one of: Red, Blue, Green.
```
*Exclusive disjunction: exactly one of the listed items*

### 31.2 Inline Lists with `any of:`

```sloj
The system supports any of: HTTP, HTTPS, FTP.
The format is any of: JSON, XML, CSV.
The user may select any of: email, SMS, push-notification.
```
*Inclusive disjunction: at least one of the listed items*

### 31.3 Inline Lists with `all of:`

```sloj
The request must include all of: a header, a body, a timestamp.
The system must support all of: authentication, authorization, logging.
The document contains all of: a title, an abstract, a body.
```
*Conjunction: all of the listed items*

### 31.4 Inline Lists with `none of:`

```sloj
The value is none of: 0, -1, "null".
The status is none of: "deleted", "archived", "suspended".
The password must contain none of: "password", "123456", "qwerty".
```
*Negated disjunction: none of the listed items*

### 31.5 Inline Lists with Full NPs

```sloj
The actor is one of: a registered user, an admin, a guest with limited privileges.
The target is one of: the primary server, the backup server, the local cache.
The source is any of: a file, a database, an external API.
```
*List items can be full noun phrases with determiners and modifiers*

### 31.6 Inline Lists with Mixed Types

```sloj
The value is one of: 3, "pippo", xld, 42, "foo".
The identifier is one of: X1, "default", 0, the current-id.
The response-code is one of: 200, 404, 500, "unknown".
```
*List items can mix numbers, strings, proper nouns, and NPs*

### 31.7 Guillemeted Inline Lists

```sloj
The value is one of: «3, "pippo", xld», or one of: «7, 8, 9».
The system requires all of: «a user, a password», and any of: «a token, a certificate».
The status is one of: «"active", "pending"», or one of: «"archived", "deleted"».
```
*Guillemets required when multiple lists in one sentence*

### 31.8 Guillemeted Lists Mid-Sentence

```sloj
The value is one of: «3, 5, 7», and the system validates it.
The color is one of: «Red, Blue», and the system displays it.
```
*Guillemets required when list is followed by more content*

### 31.9 Markdown Lists with `one of:`

```sloj
The value is one of:
- 3.
- "pippo".
- xld.

The status is one of:
- "active".
- "pending".
- "closed".
```
*Markdown format for exclusive disjunction*

### 31.10 Markdown Lists with `any of:`

```sloj
The system supports any of:
- HTTP.
- HTTPS.
- FTP.

The user may select any of:
- email.
- SMS.
- push-notification.
```
*Markdown format for inclusive disjunction*

### 31.11 Markdown Lists with `all of:`

```sloj
The request must include all of:
- a header.
- a body.
- a timestamp.

The document contains all of:
- a title.
- an abstract.
- a body.
```
*Markdown format for conjunction*

### 31.12 Markdown Lists with `none of:`

```sloj
The value is none of:
- 0.
- -1.
- "null".

The password must contain none of:
- "password".
- "123456".
- "qwerty".
```
*Markdown format for negated disjunction*

### 31.13 Markdown Lists with Full NPs

```sloj
The actor is one of:
- a registered user.
- an admin.
- a guest with limited privileges.

The target is one of:
- the primary server.
- the backup server.
- the local cache.
```
*Markdown list items can be full noun phrases*

### 31.14 Single-Item Lists

```sloj
The value is one of: X.
The status is one of: "active".
```
*Single-item lists are allowed (though unusual)*

### 31.15 Open Lists (Ellipsis)

Lists are **exhaustive** by default. To indicate an **open** list (where other values may exist), append an ellipsis (`...` or `…`) as the final element.

#### Inline Open Lists

```sloj
The state is one of: idle, running, stopped, ...
The format supports any of: JSON, XML, CSV, …
The error-code is one of: 400, 404, 500, ...
```
*Ellipsis indicates other values may exist*

#### Inline Exhaustive Lists (Default)

```sloj
The state is one of: idle, running, stopped.
The boolean is one of: true, false.
The priority is one of: low, medium, high.
```
*No ellipsis means these are ALL possible values*

#### Markdown Open Lists

```sloj
The protocol is one of:
- HTTP.
- HTTPS.
- FTP.
- ...

The color is any of:
- Red.
- Green.
- Blue.
- …
```
*Ellipsis as final list item indicates open list*

#### Markdown Exhaustive Lists (Default)

```sloj
The status is one of:
- "active".
- "pending".
- "closed".

The day is one of:
- Monday.
- Tuesday.
- Wednesday.
- Thursday.
- Friday.
- Saturday.
- Sunday.
```
*No ellipsis means these are ALL possible values*

#### Ellipsis with Period

```sloj
The value is one of:
- 1.
- 2.
- 3.
- ....
```
*Ellipsis item may optionally end with a period*

#### Open vs Exhaustive Semantics

```sloj
The HTTP-method is one of: GET, POST, PUT, DELETE.
```
*Exhaustive: ONLY these four methods are valid*

```sloj
The HTTP-method is one of: GET, POST, PUT, DELETE, ...
```
*Open: these are examples; other methods (PATCH, HEAD, etc.) may also be valid*

---

## End of Examples
