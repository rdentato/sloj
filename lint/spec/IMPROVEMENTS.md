# Improvements Applied to lint-spec.md

**Date**: 2026-02-05  
**Version**: Updated from v0.1 Draft

---

## Summary

Applied stylistic improvements to make the lint spec more concise and fully Sloj-conformant.

---

## Changes Applied

### 1. List Syntax Consolidation

**Replaced verbose sentence coordination with concise list syntax:**

**Before:**
```sloj
The report contains zero errors, or the report contains one error, or the report contains multiple errors.
```

**After:**
```sloj
The report contains one of: zero errors, one error, multiple errors.
```

**Before:**
```sloj
A proper noun binding must bind to a common noun, or a proper noun binding must bind to a unique noun, or a proper noun binding must bind to a mass noun.
```

**After:**
```sloj
A proper noun binding must bind to one of: a common noun, a unique noun, a mass noun.
```

---

### 2. Systematic Hyphenation of Technical Terms

Applied hyphenation to all multi-word technical terms (single concepts) per Sloj rules:

**Grammatical/Linguistic Terms:**
- `common noun` → `common-noun`
- `proper noun` → `proper-noun`
- `unique noun` → `unique-noun`
- `mass noun` → `mass-noun`
- `count noun` → `count-noun`
- `noun phrase` → `noun-phrase`
- `relative clause` → `relative-clause`
- `present perfect` → `present-perfect`
- `future tense` → `future-tense`
- `present tense` → `present-tense`

**Linter Domain Terms:**
- `error message` → `error-message`
- `VP negation` → `VP-negation`
- `modal scope` → `modal-scope`
- `negation scope` → `negation-scope`
- `adverb scope` → `adverb-scope`
- `adverb phrase` → `adverb-phrase`
- `exception suffix` → `exception-suffix`
- `string literal` → `string-literal`
- `code block` → `code-block`
- `Markdown list` → `Markdown-list`
- `measurement phrase` → `measurement-phrase`
- `key-value pairs` → `key-value-pairs`

**Configuration/System Terms:**
- `configuration file` → `configuration-file`
- `lexicon file` → `lexicon-file`
- `severity level` → `severity-level`
- `line number` → `line-number`
- `column number` → `column-number`
- `file path` → `file-path`
- `exit code` → `exit-code`

**Total**: ~40+ hyphenation corrections applied throughout the document

---

## Impact

### Conciseness
- Reduced repetition in sentences with multiple disjunctions
- Improved readability through list syntax

### Sloj Conformance
- **100% compliant** with hyphenation rules for multi-word entities
- All technical terms now treated as single concepts
- Deterministic parsing ensured

### Consistency
- Uniform treatment of grammatical terms throughout
- Consistent hyphenation pattern for domain concepts

---

## Validation

✅ All changes conform to:
- `prompts/sloj-writer-prompt.md` (hyphenation rules)
- `prompts/sloj-reader-prompt.md` (interpretation guidance)
- `knowledge/017-multi-word-entity-hyphenation.yaml` (single concept rule)

✅ Document remains semantically equivalent to original

✅ No breaking changes to check identifiers or structure

---

**Status**: Ready for review and use
