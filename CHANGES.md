# Sloj Project Changes

## 2026-02-05

### LLM Prompts and Linter Specification

**Major additions:**

- **LLM Prompts** (Phase 4.3 complete):
  - `prompts/sloj-reader-prompt.md` (v1.2) - Interpret Sloj requirements with validation
  - `prompts/sloj-writer-prompt.md` (v1.1) - Generate valid Sloj from natural language
  - Comprehensive test cases (47 total: 17 reader + 30 writer)
  - Test results: 100% pass rate
  - Test summary and evaluation documents

- **Linter Specification**:
  - `lint/spec/lint-spec.md` - Complete semantic checker specification in Sloj
  - 42 check types across 10 categories
  - Fully Sloj-conformant with systematic hyphenation
  - Review documentation and improvements log

- **Knowledge Base**:
  - Entry 017: Multi-word entity hyphenation rules
  - Entry 018: Capitalization and determiner noun category rules
  - Entry 019: Linter check organization
  - Entry 020: Sentence start constraint (fundamental structural rule)

- **Specification Updates**:
  - Section 14: Lexical Rules (hyphenation, capitalization, morphology)
  - Updated examples with proper hyphenation
  - Enhanced sentence coordination rules

- **Project Management**:
  - Updated `PLAN.md`: Task 4.3 (LLM usage) marked complete
  - Updated `AGENTS.md`: Clarified version control handling for evaluations/
  - Added evaluation documents for LLM prompt design approaches

**Commit:** `c5e1004` (26 files changed, +6,567 lines)

---

## Earlier Changes

See `ARCHIVE.md` for historical changes and session notes.
