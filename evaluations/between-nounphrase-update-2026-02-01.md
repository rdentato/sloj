# Between NounPhrase Update

**Date**: 2026-02-01  
**Change**: Updated `between` construct to use `NounPhrase` instead of `Number and Number`  
**Status**: ✅ Complete

---

## Summary

Updated the `between` range comparison construct to use `between NounPhrase` instead of `between Number and Number`. This change improves consistency with Sloj's symbolic coordination philosophy and enforces typed comparisons.

---

## Rationale

### Problems with `between Number and Number`:
1. **Inconsistent**: Uses plain English `and` instead of symbolic `&`
2. **Untyped**: Allows bare numbers without units (e.g., "between 30 and 40" - 30 what?)
3. **Inflexible**: Doesn't naturally extend to measurement phrases

### Benefits of `between NounPhrase`:
1. **Consistent**: Uses `&` for coordination like all other NP coordination in Sloj
2. **Typed**: Requires units/nouns, making specifications clearer
3. **Flexible**: Handles single NPs, coordinated NPs, and measurement phrases
4. **Composable**: Works naturally with measurement phrases (Task 2.7)

---

## Valid Forms

### Single NP
```sloj
"The level is between the limits."
```
*Single noun phrase representing a range*

### Coordinated NPs
```sloj
"The level is between the min-limit & the max-limit."
```
*Two noun phrases coordinated with `&`*

### Measurement Phrases (requires Task 2.7)
```sloj
"The response-time is between 30 seconds & 40 seconds."
```
*Two measurement phrases coordinated with `&`*

### Coordinated Numbers with Unit
```sloj
"The temperature is between «0 & 100» degree-celsius."
```
*Coordinated numbers in guillemets with a unit noun*

---

## Invalid Forms

### Bare Numbers
```sloj
"The level is between 30 & 40."  ❌
```
*Bare numbers without noun/unit are not allowed*

### Using `and` instead of `&`
```sloj
"The level is between 30 and 40."  ❌
```
*Plain English `and` is not allowed (inconsistent with Sloj)*

---

## Grammar Change

### Before:
```ebnf
Comparison := ...
            | 'between' Number 'and' Number
```

### After:
```ebnf
Comparison := ...
            | 'between' NounPhrase
```

Where `NounPhrase` can be:
- A single NP
- Coordinated with `&`: `NP & NP`
- Include measurement phrases (when implemented)

---

## Spec Changes

### Section 6.6 Range Comparisons

**Before:**
| Form | Example |
|------|---------|
| NP `is between` Number `and` Number | "The response-time is between 10 and 50." |

**After:**
| Form | Example |
|------|---------|
| NP `is between` NP | "The level is between the limits." |
| NP `is between` NP `&` NP | "The level is between the min-limit & the max-limit." |

**Notes added:**
- `between` is followed by a noun phrase (NP), which may be coordinated with `&`
- Bare numbers are not allowed; use measurement phrases
- Full support requires measurement phrases (Task 2.7)

---

## Examples Changes

### Before:
```sloj
"The response-time is between 10 and 50."
"The temperature is between 0 and 100."
```

### After:
```sloj
"The level is between the limits."
"The level is between the min-limit & the max-limit."
"The temperature is between «0 & 100» degree-celsius."
```

---

## Consistency with Sloj Philosophy

This change aligns with Sloj's core principles:

| Principle | How `between NounPhrase` Supports It |
|-----------|--------------------------------------|
| **Symbolic Coordination** | Uses `&` instead of plain English `and` |
| **Explicit Scope** | Guillemets can group coordinated numbers: `«30 & 40»` |
| **Deterministic Parsing** | Clear NP structure, no ambiguity |
| **Typed Specifications** | Requires units/nouns, not bare numbers |

---

## Dependency on Measurement Phrases

Full `between` functionality requires **measurement phrases (Task 2.7)** to express:
```sloj
"The response-time is between 30 seconds & 40 seconds."
"The temperature is between 0 degree-celsius & 100 degree-celsius."
```

Until measurement phrases are implemented, `between` works with:
- Existing noun phrases: "the limits"
- Coordinated NPs: "the min & the max"
- Workarounds: "between «30 & 40» degrees" (if "degrees" is a defined noun)

---

## Files Modified

1. **`spec/sloj-spec-doc.md`**
   - Section 6.6: Updated table and notes

2. **`spec/sloj-spec-grammar.bnf`**
   - Changed `'between' Number 'and' Number` to `'between' NounPhrase`
   - Added comment explaining the requirement

3. **`spec/sloj-examples.md`**
   - Section 17.6: Updated examples to show valid forms
   - Added note about bare numbers being invalid

4. **`evaluations/comparisons-addition-2026-02-01.md`**
   - Updated grammar section
   - Updated examples section
   - Added design note explaining the decision

---

## Conclusion

The `between NounPhrase` approach is more consistent with Sloj's design principles, enforces typed comparisons, and provides better composability with future features like measurement phrases.

**Change complete and documented.**
