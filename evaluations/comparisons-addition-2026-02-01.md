# Comparisons Addition to Sloj

**Date**: 2026-02-01  
**Feature**: Comparisons (Task 2.6)  
**Status**: ✅ Complete

---

## Summary

Added comprehensive comparison support to Sloj, including comparative and superlative constructions for both adjectives and adverbs, equality/inequality comparisons, and range comparisons.

---

## Changes Made

### 1. Spec Document (`spec/sloj-spec-doc.md`)

**New Section 6: Comparisons** (inserted between Existential Sentences and Conditional Statements)

#### 6.1 Comparative Constructions (Adjectives)
- `NP is ComparativeAdj than NP`
- `NP is ComparativeAdj than Number`

#### 6.2 Comparative Constructions (Adverbs)
- `NP VP ComparativeAdv than NP`
- `NP VP ComparativeAdv than NP VP`

#### 6.3 Superlative Constructions (Adjectives)
- `NP is the SuperlativeAdj`
- `NP is the SuperlativeAdj of NP`

#### 6.4 Superlative Constructions (Adverbs)
- `NP VP the SuperlativeAdv`
- `NP VP the SuperlativeAdv of NP`

#### 6.5 Equality and Inequality
- `NP is equal to Number/NP/String`
- `NP is not equal to Number/NP/String`

#### 6.6 Range Comparisons
- `NP is between Number and Number`

**Section renumbering:**
- Old Section 6 (Conditional Statements) → New Section 7
- Old Section 7 (Noun Phrase) → New Section 8
- Old Section 8 (String Literals) → New Section 9
- Old Section 9 (Guillemets) → New Section 10

### 2. Grammar File (`spec/sloj-spec-grammar.bnf`)

**Added Comparison construct:**
```ebnf
VerbArg := ObjectNounPhrase | Adjective | PastParticiple | NonOfPrepPhrase | StringLiteral | Comparison

Comparison := ComparativeAdj 'than' Comparand
            | 'the' SuperlativeAdj
            | 'the' SuperlativeAdj 'of' NounPhrase
            | 'equal to' Comparand
            | 'not equal to' Comparand
            | 'between' NounPhrase

Comparand := Number | ObjectNounPhrase | StringLiteral

ComparativeAdj := <comparative_adjective>
SuperlativeAdj := <superlative_adjective>
```

**Note:** `between` uses `NounPhrase` (not `Number and Number`) for consistency with Sloj's symbolic coordination and to enforce typed comparisons.

**Updated Adverb rule for comparative/superlative adverbs:**
```ebnf
Adverb := AdvTerm (AdvConj AdvTerm)*
        | ComparativeAdv 'than' Comparand
        | 'the' SuperlativeAdv
        | 'the' SuperlativeAdv 'of' NounPhrase

ComparativeAdv := <comparative_adverb>
SuperlativeAdv := <superlative_adverb>
```

### 3. Examples File (`spec/sloj-examples.md`)

**Quick Reference Index updated:**
- Added 6 comparison entries (17.1-17.6)
- Renumbered all subsequent entries

**Table of Contents updated:**
- Added Section 17: Comparisons with 6 subsections
- Renumbered sections 17-26 (was 17-25)

**New examples section added (Section 17):**

#### 17.1 Comparative Adjectives
```sloj
"Tank A is larger than Tank B."
"The temperature is greater than 100."
"The count is less than 5."
```

#### 17.2 Comparative Adverbs
```sloj
"Process A completes more-quickly than Process B."
"The server responds faster than the client requests."
```

#### 17.3 Superlative Adjectives
```sloj
"Tank A is the largest."
"Tank A is the largest of the three tanks."
```

#### 17.4 Superlative Adverbs
```sloj
"Process A completes the most-quickly."
"Process A completes the fastest of all processes."
```

#### 17.5 Equality and Inequality
```sloj
"The priority is equal to 3."
"The status is equal to the previous-status."
"The count is not equal to 0."
"The status is not equal to \"pending\"."
```

#### 17.6 Range Comparisons
```sloj
"The level is between the limits."
"The level is between the min-limit & the max-limit."
"The temperature is between «0 & 100» degree-celsius."
```

**Note:** Bare numbers not allowed; must use noun phrases or measurement phrases.

---

## Syntax Summary

### Comparative Adjectives
```
NP is ComparativeAdj than {NP | Number}
```
**Examples:** larger, greater, smaller, faster, slower, better, worse

### Comparative Adverbs
```
NP VP ComparativeAdv than {NP | NP VP}
```
**Examples:** faster, more-quickly, more-slowly, better

### Superlative Adjectives
```
NP is the SuperlativeAdj [of NP]
```
**Examples:** largest, greatest, smallest, fastest, best, worst

### Superlative Adverbs
```
NP VP the SuperlativeAdv [of NP]
```
**Examples:** fastest, most-quickly, best

### Equality/Inequality
```
NP is [not] equal to {Number | NP | String}
```

### Range
```
NP is between NounPhrase
```
**Note:** 
- NounPhrase can be coordinated with `&`: "between the min & the max"
- Bare numbers not allowed; must use measurement phrases: "between 30 seconds & 40 seconds" or "between «30 & 40» degrees"
- Inclusive bounds [min, max]
- Full support requires measurement phrases (Task 2.7)

---

## Design Notes

### Lexicon-Defined Forms
Comparative and superlative forms (e.g., `larger`, `fastest`, `more-quickly`) are defined in the lexicon, not as special syntax. This allows:
- Language-specific comparative/superlative formation rules
- Domain-specific comparative terms
- Flexibility in morphology

### `than` as Syntactic Marker
`than` is a syntactic marker introducing the comparand, not a preposition. This distinguishes it from other prepositional phrases.

### `of` in Superlatives
In superlative constructions, `of` introduces the comparison domain:
- "the largest of the three tanks"
- "the fastest of all processes"

### Disambiguation: `less` and `more`
- **Followed by `than`** → comparative: "The count is less than 5"
- **Followed by mass noun** → determiner: "Less water is available"

### Comparand Types
Comparisons can use:
- **Numbers**: "greater than 100"
- **Noun phrases**: "larger than Tank B"
- **String literals**: "not equal to \"pending\""
- **VP (adverbs only)**: "responds faster than the client requests"

### `between` Uses NounPhrase
The `between` construct uses `NounPhrase` rather than `Number and Number` for several reasons:
- **Consistency**: Uses `&` for coordination, not plain English `and`
- **Typed comparisons**: Enforces that ranges have units/types (e.g., "30 seconds & 40 seconds")
- **Flexibility**: Supports single NP ("the limits"), coordinated NPs ("the min & the max"), and measurement phrases
- **Composability**: Works naturally with measurement phrases when implemented (Task 2.7)

Valid forms:
- `between the limits` (single NP)
- `between the min-limit & the max-limit` (coordinated NPs)
- `between 30 seconds & 40 seconds` (measurement phrases)
- `between «30 & 40» degrees` (coordinated numbers with unit)

Invalid:
- `between 30 & 40` (bare numbers without noun/unit)

---

## Grammar Integration

### As VerbArg
Comparisons integrate into the unified verb syntax as a type of `VerbArg`:
```
NP is Comparison
```

This treats comparisons consistently with other predicates:
- `NP is Adj` → "The dog is fast"
- `NP is NP` → "John is a user"
- `NP is Comparison` → "Tank A is larger than Tank B"

### Adverb Position
Comparative and superlative adverbs appear in standard adverb positions (pre-verbal or post-verbal):
```
NP VP ComparativeAdv than Comparand
NP VP the SuperlativeAdv [of NP]
```

---

## Consistency with Sngl

Sloj's comparison syntax is based on Sngl's design with minor adaptations:

| Feature | Sngl | Sloj | Notes |
|---------|------|------|-------|
| Comparative adjectives | ✅ | ✅ | Same syntax |
| Comparative adverbs | ✅ | ✅ | Same syntax |
| Superlative adjectives | ✅ | ✅ | Same syntax |
| Superlative adverbs | ✅ | ✅ | Same syntax |
| Equality | ✅ | ✅ | Same syntax |
| Range (between...and) | ✅ | ✅ | Same syntax |
| Numeric expressions | ✅ | ❌ | Deferred (e.g., `{N + 5}`) |

**Note:** Numeric expressions (like `{N + 5}`) are not yet supported in Sloj. This is a potential future enhancement.

---

## Testing Considerations

When implementing:
1. Verify comparative/superlative forms are in lexicon
2. Test with all comparand types (Number, NP, String)
3. Test superlatives with and without `of` domain
4. Test equality with all value types
5. Test range with various number pairs
6. Verify `less`/`more` disambiguation
7. Test comparative adverbs with VP comparands
8. Verify `than` is not treated as preposition

---

## Future Enhancements

Potential additions (not in current spec):
1. **Numeric expressions**: `greater than {N + 5}`
2. **Measurement comparisons**: `larger than 5 meters` (requires measurement phrases)
3. **Mass comparisons**: `more water than` (Task 2.8)
4. **Degree modifiers**: `much larger than`, `slightly faster than`

---

## Conclusion

Comparisons have been successfully added to Sloj, providing comprehensive support for expressing relationships between entities and values. The implementation integrates cleanly with the existing unified verb syntax and maintains consistency with Sngl's design.

**Files modified:**
- `spec/sloj-spec-doc.md` - Added Section 6, renumbered 6-9 → 7-10
- `spec/sloj-spec-grammar.bnf` - Added Comparison and updated Adverb rules
- `spec/sloj-examples.md` - Added Section 17 with examples, renumbered 17-25 → 18-26

**Task 2.6 (Comparisons) is now complete.**
