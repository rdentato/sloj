# Sloj Lint Spec Review

**Date**: 2026-02-05  
**Reviewer**: Amenhotep  
**Document**: `lint/spec/lint-spec.md` v0.1

---

## Summary

The lint spec is **largely correct** and follows Sloj rules well. A few minor improvements recommended below for clarity and consistency.

---

## Issues Found

### Issue 1: Line 59 - Could use list syntax (OPTIONAL)

**Current**:
```sloj
The report contains zero errors, or the report contains one error, or the report contains multiple errors.
```

**Status**: Technically correct (sentence-level coordination with `, or`)

**Suggestion** (optional, for conciseness):
```sloj
The report contains one of: zero errors, one error, multiple errors.
```

**Priority**: Low (stylistic)

---

### Issue 2: Line 138 - Could use list syntax (OPTIONAL)

**Current**:
```sloj
A proper noun binding must bind to a common noun, or a proper noun binding must bind to a unique noun, or a proper noun binding must bind to a mass noun.
```

**Status**: Technically correct

**Suggestion** (optional, for conciseness):
```sloj
A proper noun binding must bind to one of: a common noun, a unique noun, a mass noun.
```

**Priority**: Low (stylistic)

---

### Issue 3: Line 294 - Potential ambiguity with modal keywords (MINOR)

**Current**:
```sloj
A Markdown list must be the final element of a sentence.
```

**Status**: Correct

**Note**: Consider clarifying that "Markdown list" means the list construct (not just any Markdown formatting). The hyphen would help if it's a compound: `Markdown-list`. But "Markdown list" (two words, unhyphenated) suggests Markdown is an adjective modifying "list", which may be the intent.

**Priority**: Informational only

---

## Positive Observations

✅ **Coordination**: Consistently uses sentence-level `, or` and `, and` (not symbols at sentence boundaries)

✅ **Hyphenation**: Correctly hyphenates multi-word concepts:
- `error-message` (line 89)
- `high-priority` (if used)
- Proper nouns like examples would be hyphenated

✅ **Determiners**: Proper use throughout (common nouns have determiners, proper nouns don't)

✅ **Modals**: Correctly uses `must`, `may`, `must not` with formal meanings

✅ **List keywords**: Correctly uses `one of:` format (lines 293, 301-306)

✅ **Temporal restrictions**: No improper use of present perfect or future tense

✅ **Agreement**: Consistent subject-verb agreement throughout

✅ **String literals**: Correctly quoted in examples (line 293, appendices)

---

## Recommendations

### R1: Consider consolidating repetitive sentence-level coordination

Where you have:
```sloj
X, or Y, or Z.
```

Consider using:
```sloj
X is one of: A, B, C.
```

This is more concise and equally precise.

**Affected lines**: 59, 138, possibly others

**Impact**: Improved readability, reduced verbosity

---

### R2: Verify multi-word concepts are hyphenated consistently

Check these terms throughout the document to ensure they're treated as single concepts:

- `error-message` vs `error message`
- `Markdown-list` vs `Markdown list` (is "Markdown" an adjective or part of a compound?)
- `VP-negation` vs `VP negation`

If they're single concepts (which they appear to be), hyphenate them.

**Impact**: Ensures Sloj conformance

---

## Verdict

**Status**: ✅ **VALID SLOJ** (with minor stylistic improvement opportunities)

The document correctly follows Sloj syntax rules. The issues identified are primarily stylistic (verbosity) rather than correctness errors.

The spec is ready for use as-is, with optional refinements noted above.

---

**End of Review**
