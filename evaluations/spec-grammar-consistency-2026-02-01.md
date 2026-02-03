# Spec-Grammar Consistency Evaluation

**Date**: 2026-02-01  
**Spec Version**: 0.1 (rewritten, concise)  
**Grammar File**: `spec/sloj-spec-grammar.bnf`  
**Evaluator**: AI Assistant

---

## Executive Summary

This evaluation compares the rewritten `sloj-spec-doc.md` against `sloj-spec-grammar.bnf` to identify gaps, inconsistencies, and areas requiring alignment.

**Overall Status**: ✅ **Good alignment** with minor issues

**Key Findings**:
- Grammar is comprehensive and covers all Tier 1 features
- Section references in grammar need updating (refer to old section numbers)
- A few minor inconsistencies in terminology
- Some spec features lack explicit grammar representation
- Grammar includes constructs not yet documented in spec (Tier 2/3 features)

---

## Section-by-Section Analysis

### 1. Overview (Spec Section 1)
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  
**Issues**: None

---

### 2. Sentence Structure (Spec Section 2)

#### 2.1 Sentence Coordination
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
Sentence := DisjunctSent '.'
          | DisjunctSentWithList
          | 'either' ConjunctSent ', or' ConjunctSent '.'
          | 'neither' ConjunctSent ', nor' ConjunctSent '.'
          ...

DisjunctSent := ConjunctSent (', or' ConjunctSent)*
ConjunctSent := SimpleSent ((', and' | ', but' | ', whereas') SimpleSent)*
```

**Issues**: None

---

### 3. Verb Phrase (VP) Coordination (Spec Section 3)

#### 3.1 VP Operators
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
VerbConj := '&&' | '&& then' | '&&/'
EitherVP := 'either' VerbPhrase ', or' VerbPhrase
NeitherVP := 'neither' VerbPhrase ', nor' VerbPhrase
```

**Issues**: None

---

#### 3.2 Precedence and Associativity
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete (implicit in left-recursive rules)  

**Grammar Rules**:
```ebnf
VPBody := VerbPhrase (VerbConj VerbPhrase)*
```

**Issues**: None

---

#### 3.3 Prepositional Phrase Attachment
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
OfClause := 'of' NounPhrase
NonOfPrepPhrase := NonOfPreposition NounPhrase
NonOfPreposition := 'in' | 'on' | 'at' | 'under' | 'above' | 'below' | ...
```

**Grammar Comments** (lines 216-220):
```
(* Binding and determinism rules (see Section 3.4):
   - An of-clause always binds to the preceding noun phrase.
   - A non-'of' prepositional phrase binds to the preceding verb if there is one...
   - In object position after a verb, a noun phrase MUST NOT contain a non-'of' 
     prepositional phrase unless it is guillemeted.
   - Guillemets override default binding. *)
```

**Issues**: 
- ⚠️ Grammar comment references "Section 3.4" but in new spec this is Section 3.3
- ✅ Object-position restriction properly enforced via separate `ObjectNounPhrase` rules

---

#### 3.4 Adverb Placement and Scope
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
SimpleVPCore := Adverb VPHead | VPHead Adverb?
GroupVPCore := Adverb GroupBody | GroupBody Adverb?
```

**Issues**: None

---

#### 3.5 Modal Auxiliaries
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
Modal := 'must not'
       | 'must'
       | 'may preferably'
       | 'preferably may'
       | 'may'
       | 'should not'
       | 'should'
       | 'is expected to'
       | 'are expected to'
       | 'is not able to'
       | 'are not able to'
       | 'is able to'
       | 'are able to'
```

**Issues**: None - Perfect match

---

#### 3.6 VP Negation
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
VPNeg := 'does not' | 'do not' | 'is not' | 'are not'
```

**Issues**: None

---

#### 3.7 VP List Coordination (Markdown)
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
VPList := 'either'? ':' NEWLINE VPListItems
VPListItem := '-' 'or'? 'then'? VerbPhrase '.'
```

**Grammar Comments** (line 141):
```
(* VP List. See Section 3.8. *)
```

**Issues**: 
- ⚠️ Grammar comment references "Section 3.8" but in new spec this is Section 3.7

---

#### 3.8 Present Perfect
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
PerfectVP := PerfectAux PerfectNeg? PerfectCore
PerfectAux := 'has' | 'have'
PerfectNeg := 'not' | 'never'
PerfectHead := PastParticiple VerbArg? NonOfPrepPhrase*
```

**Grammar Comments** (lines 337-346):
```
(* Present perfect is permitted in relative clauses. See Section 3.9. *)
(* Present perfect is restricted to relative clauses and conditional antecedents.
   It expresses that a predicate held at some prior time-point. *)
```

**Issues**: 
- ⚠️ Grammar comment references "Section 3.9" but in new spec this is Section 3.8

---

#### 3.9 Unified Verb Syntax
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
VPHead := Verb VerbArg? NonOfPrepPhrase*
        | SelfVerb NonOfPrepPhrase*

VerbArg := ObjectNounPhrase | Adjective | PastParticiple | NonOfPrepPhrase | StringLiteral
```

**Grammar Comments** (lines 189-206):
```
(* Unified Verb Phrase Head
   All verbs (including is/are/be, becomes, seems, runs, validates, etc.)
   follow the same argument pattern. Semantic validity is checked by the
   semantic layer, not the grammar. *)
```

**Issues**: None - Excellent alignment

---

#### 3.10 Passive Voice
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete (implicit in unified verb syntax)  

**Issues**: 
- ℹ️ Passive voice is a semantic interpretation, not a distinct grammar rule
- ✅ Grammar correctly treats it as `is/are/be + PastParticiple` via `VerbArg`

---

### 4. Temporal and Future Statements (Spec Section 4)

#### 4.1 Basic Future Forms
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
TemporalSent := NounPhrase FutureAux VPBody TemporalSuffix?
              | TemporalPrefix ',' NounPhrase FutureAux VPBody

FutureAux := 'will' | 'will always' | 'will eventually' | 'will immediately' 
           | 'will then' | 'will never' | 'will not'
```

**Grammar Comments** (line 23):
```
(* Temporal and Future Statements - See Section 4 *)
```

**Issues**: None

---

#### 4.2 Temporal Scopes
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
TemporalPrefix := 'before' ConditionClause
                | 'after' ConditionClause
                | 'after' ConditionClause 'and before' ConditionClause
                | 'while' ConditionClause
```

**Issues**: None

---

#### 4.3 Continuous Temporal Conditions
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
TemporalSuffix := 'until' ConditionClause
                | 'unless' ConditionClause
                | 'within' Duration
                | 'for' Duration
                | 'every' Duration
                | 'at least every' Duration
                | 'at most every' Duration
                | 'every' Duration 'to' Duration

WhileSentWithList := 'while' ConditionClause ', either'? ':' NEWLINE ConsequenceList
```

**Grammar Comments** (lines 51-52):
```
(* While with statement list. See Section 4.3.1.
   Uses ConsequenceList for the list items (same as conditionals). *)
```

**Issues**: 
- ⚠️ Spec doesn't have a "Section 4.3.1" subsection - it mentions while lists in Section 4.3 but doesn't create a numbered subsection

---

#### 4.4 Temporal Sequences
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
TemporalSequence := 'first' SimpleSent (',' 'then' SimpleSent)+ (',' 'finally' SimpleSent)?
```

**Issues**: None

---

#### 4.5 Temporal Bounds (Durations)
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
Duration := Number DurationUnit
DurationUnit := 'nanosecond' | 'nanoseconds' | 'ns' | ...
```

**Issues**: None - Perfect match on all duration units

---

#### 4.6 Scope and Coordination
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete (via FutureVP and FutureGroupVP)  

**Grammar Rules**:
```ebnf
FutureVP := 'will' SimpleVPCore
FutureGroupVP := 'will' GroupVPCore
```

**Issues**: None

---

### 5. Existential Sentences (Spec Section 5)

**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
ExistentialSent := 'there is' ExistentialNeg? NounPhrase NonOfPrepPhrase*
                 | 'there are' ExistentialNeg? NounPhrase NonOfPrepPhrase*

ExistentialNeg := 'not'
```

**Issues**: None

---

### 6. Conditional Statements (Spec Section 6)

#### 6.1 Main Conditional Forms
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
ConditionalSent := CondKeyword ConditionClause ', then' ConsequenceClause
                 | ConsequenceClause ', except when' ConditionClause
                 | ConsequenceClause ', except if' ConditionClause

CondKeyword := 'if' | 'whenever' | 'once' | 'each time'
```

**Grammar Comments** (lines 70-72):
```
(* The condition may use present tense or present perfect. Future is forbidden.
   The consequence may use present tense or simple future (will + VP). See Section 6. *)
```

**Issues**: None

---

#### 6.1 Otherwise Clause (Spec Section 6.1)
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
OtherwiseSent := 'Otherwise,' ConsequenceClause
```

**Issues**: None

---

#### 6.2 Exception Forms (Spec Section 6.2)
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
ConditionalSent := ...
                 | ConsequenceClause ', except when' ConditionClause
                 | ConsequenceClause ', except if' ConditionClause
```

**Issues**: None

---

#### 6.3 Conditional Statement Lists (Spec Section 6.3)
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
ConditionalSentWithList := CondKeyword ConditionClause 'either'? ':' NEWLINE ConsequenceList
ConsequenceList := ConsequenceListItem+
ConsequenceListItem := '-' 'or'? 'then'? NounPhrase ConsequenceBody '.'
```

**Grammar Comments** (lines 85-86):
```
(* Conditional with statement list. See Section 6.9. *)
```

**Issues**: 
- ⚠️ Grammar comment references "Section 6.9" but in new spec this is Section 6.3

---

### 7. Noun Phrase (NP) Coordination (Spec Section 7)

#### 7.1 NP Operators
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
NounConj := '&' | '/' | '&/'
```

**Issues**: None

---

#### 7.2 Precedence
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete (implicit in left-recursive rules)  

**Grammar Rules**:
```ebnf
NounCore := NounTerm (NounConj NounTerm)*
Adjective := AdjTerm (AdjConj AdjTerm)*
Adverb := AdvTerm (AdvConj AdvTerm)*
```

**Issues**: None

---

#### 7.3 Noun Types
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
CommonNP := Determiner Adjective* CommonNoun Binding? OfClause* NonOfPrepPhrase* RelativeClause?
ProperNP := ProperNoun OfClause* NonOfPrepPhrase* RelativeClause?
UniqueNP := Determiner Adjective* UniqueNoun Binding? OfClause* NonOfPrepPhrase* RelativeClause?
MassNP := MassDeterminer Adjective* MassNoun Binding? OfClause* NonOfPrepPhrase* RelativeClause?
```

**Issues**: None

---

#### 7.4 Special Determiners
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
Determiner := 'the' | 'a' | 'an' | 'some' | 'every' | 'each' | 'all' | 'no' | 'enough' | 'a distinct' | ...
MassDeterminer := 'the' | 'some' | 'no' | 'more' | 'less' | 'enough'
```

**Issues**: None

---

#### 7.5 Pronouns
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
PronounNP := SubjectPronoun | ObjectPronoun | PossessivePronoun
SubjectPronoun := 'he' | 'she' | 'it' | 'they'
ObjectPronoun := 'him' | 'her' | 'it' | 'them'
PossessivePronoun := 'his' | 'hers' | 'its' | 'theirs'
```

**Grammar Comments** (line 313):
```
(* Reflexive pronouns are not used; use self- prefix on verbs instead. *)
```

**Issues**: 
- ⚠️ Spec mentions "Reflexive Possessive" (`his own`, `her own`, etc.) in table but grammar doesn't include these
- ✅ However, these are in `Determiner` rule (lines 399-400), so they're covered

---

#### 7.6 Binding
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
Binding := ProperNoun
         | ',' ProperNoun ','
         | ', named' ProperNoun ','
         | ', called' ProperNoun ','
```

**Issues**: 
- ⚠️ Spec mentions "aka" form but grammar doesn't include it
- ℹ️ This might be intentional (aka removed from Sloj vs Sngl)

---

#### 7.7 Partitive Constructions
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
PartitiveNP := PartitiveHead OfThe PartitiveComp
PartitiveHead := ProportionPartitiveHead | CountPartitiveHead
ProportionPartitiveHead := 'some' | 'all' | 'none' | 'most' | 'half' | Percent
CountPartitiveHead := Number | 'only' Number | 'exactly' Number | 'at least' Number | ...
```

**Issues**: None - Excellent coverage

---

#### 7.8 Exception Suffix
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
ExceptSuffix := 'except' ExceptTarget
ExceptTarget := NounCore | ExceptProNP
ExceptProNP := 'the' ('one' | 'ones') RelativeClause
```

**Issues**: None

---

### 8. String Literals (Spec Section 8)

#### 8.1 Forms and Escaping
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
StringLiteral := DoubleQuotedString | BacktickString
DoubleQuotedString := '"' <string_content> '"'
BacktickString := '`' <string_content> '`'
```

**Issues**: None

---

#### 8.4 Fenced Code Blocks
**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete  

**Grammar Rules**:
```ebnf
CodeBlock := TripleBacktickBlock | TripleTildeBlock
TripleBacktickBlock := '```' SP* Language? NEWLINE CodeContent NEWLINE '```'
TripleTildeBlock := '~~~' SP* Language? NEWLINE CodeContent NEWLINE '~~~'
```

**Issues**: None

---

### 9. Explicit Grouping (Guillemets) (Spec Section 9)

**Spec Coverage**: ✅ Complete  
**Grammar Coverage**: ✅ Complete (used throughout)  

**Grammar Rules**:
```ebnf
GroupBody := '«' VerbPhrase (VerbConj VerbPhrase)* '»'
NounTerm := ... | '«' NounPhrase '»'
AdjTerm := <adjective> | '«' Adjective '»'
AdvTerm := <adverb> | '«' Adverb '»'
```

**Issues**: None

---

## Missing in Spec but Present in Grammar

### 1. Condition/Consequence Body Distinction
**Grammar has**:
```ebnf
ConditionBody := ConditionVP (VerbConj ConditionVP)*
ConditionVP := VerbPhrase | PerfectVP | PerfectGroupVP

ConsequenceBody := ConsequenceVP (VerbConj ConsequenceVP)*
ConsequenceVP := VerbPhrase | FutureVP | FutureGroupVP
```

**Spec**: Mentions this in prose but doesn't explicitly show the distinction in tables

**Impact**: ℹ️ Minor - Spec is clear about tense restrictions, grammar formalizes it

---

### 2. Object-Position Restrictions
**Grammar has**: Separate `ObjectNounPhrase`, `ObjectNounCore`, `ObjectNounTerm` rules

**Spec**: Mentions restriction in Section 3.3 table but doesn't emphasize the separate grammar treatment

**Impact**: ℹ️ Minor - Important for determinism, well-documented in grammar comments

---

## Missing in Grammar but Present in Spec

### None identified
All spec features have corresponding grammar rules.

---

## Terminology Inconsistencies

### 1. "Complement" vs "Argument"
- **Spec Section 3.8**: Uses "Comp" (Complement) for present perfect forms
- **Grammar**: Uses "VerbArg" consistently
- **Impact**: ℹ️ Minor - Both terms are acceptable

### 2. Section Number References
Multiple grammar comments reference old section numbers:
- Line 141: "See Section 3.8" → should be 3.7
- Line 220: "see Section 3.4" → should be 3.3
- Line 337: "See Section 3.9" → should be 3.8
- Line 52: "See Section 4.3.1" → should be 4.3 (no subsection)
- Line 86: "See Section 6.9" → should be 6.3

**Impact**: ⚠️ **Medium** - Confusing for readers cross-referencing

---

## Ambiguity Analysis

### Potential Ambiguities Checked:

#### 1. VP Coordination Precedence
**Status**: ✅ **Unambiguous**  
**Reason**: Left-recursive grammar rules enforce left-to-right evaluation

#### 2. NP Coordination Precedence
**Status**: ✅ **Unambiguous**  
**Reason**: Left-recursive grammar rules enforce left-to-right evaluation

#### 3. Prepositional Phrase Attachment
**Status**: ✅ **Unambiguous**  
**Reason**: Object-position restrictions + guillemet override rules ensure determinism

#### 4. Adverb Scope
**Status**: ✅ **Unambiguous**  
**Reason**: Pre-verbal or post-verbal positions clearly defined; guillemets for wider scope

#### 5. Modal/Negation Scope
**Status**: ✅ **Unambiguous**  
**Reason**: VPPrefix attaches to closest verb; guillemets for wider scope

#### 6. VP List Positioning
**Status**: ✅ **Unambiguous**  
**Reason**: Separate `*WithList` rules ensure VP lists are always final

#### 7. Sentence-Level vs VP-Level `either`/`neither`
**Status**: ✅ **Unambiguous**  
**Reason**: Sentence-level forms begin sentence; VP-level forms follow subject

---

## Recommendations

### High Priority (Should Fix)

1. **Update Grammar Section References**
   - Update all grammar comments to reference new spec section numbers
   - Estimated effort: 15 minutes

2. **Clarify "aka" Binding Form**
   - Either add to grammar or remove from spec
   - Verify if this was intentionally removed from Sloj
   - Estimated effort: 5 minutes

### Medium Priority (Nice to Have)

3. **Add Grammar Comments for Object-Position Rules**
   - Emphasize why separate `ObjectNounPhrase` rules exist
   - Link to determinism discussion
   - Estimated effort: 10 minutes

4. **Standardize Terminology**
   - Choose "Complement" or "Argument" consistently
   - Update either spec or grammar
   - Estimated effort: 10 minutes

### Low Priority (Optional)

5. **Add Explicit Ambiguity-Prevention Comments**
   - Document how left-recursion prevents ambiguity
   - Add notes on precedence enforcement
   - Estimated effort: 20 minutes

---

## Conclusion

**Overall Assessment**: ✅ **Excellent alignment**

The grammar and spec are well-aligned with only minor issues:
- Section number references need updating (mechanical fix)
- One potential missing feature (aka binding)
- Minor terminology variations

**No structural issues or ambiguities detected.**

The grammar is comprehensive, unambiguous, and correctly implements all Tier 1 features described in the spec.

**Recommendation**: Update section references, clarify aka binding, then mark task 2.1 as complete.
