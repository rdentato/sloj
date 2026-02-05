# Project Notes: Sloj

**CRITICAL PROTOCOL**: 
1. Always refer to PLAN.md before starting work.
2. **NEVER unblock a task (change `[!]` to `[ ]`) yourself.** Only the user can unblock tasks.
3. You cannot work on blocked phases/steps unless the user explicitly unblocks them.
4. **NEVER mark a task as completed (`[x]`) yourself.** Always ask the user to confirm completion first.

## Current Context

- Spec is at v0.1 Draft status
- Phase 2 (Syntax specification) is **COMPLETE**
- Phase 4.3 (LLM usage) is **COMPLETE** - prompts at v1.1 (writer) and v1.2 (reader)
- Phase 4 (Tooling) started with lint subproject
- Grammar file: `spec/sloj-spec-grammar.bnf`
- Persistent knowledge stored in `knowledge/` (20 entries)

## Open Questions

- Formal logic mapping (FOL, temporal logic, operational semantics) on hold

## Deferred Features

- **Sentence negation (task 2.9)**: VP negation (`does not`, `do not`) covers most use cases. Sentence-level negation (`it is false that S`) deferred.
- **Let-binding syntax**: Proposed `Let Name be NP, then...` syntax discussed and rejected. Current proper noun binding is sufficient; adding keyword would violate minimalism without solving real problem.

## Future Enhancements

- **Claude Code skills**: May want to transform sloj-reader-prompt.md and sloj-writer-prompt.md into auto-activated skills for seamless workflow.

## Known Issues / Blockers

- Phase 3 (Formal logic mapping) blocked until syntax spec complete
- Phase 4 (Tooling) partially unblocked (lint subproject)
- Phase 5 (Documentation) blocked

## Session Log

### 2026-02-05 (Session 3)
**Worked on:**
- Lint spec Sloj conformance review
- Prompt enhancements (sentence structure rule)
- Systematic hyphenation corrections

**Completed:**
- Reviewed `lint/spec/lint-spec.md` against sloj-reader-prompt.md and sloj-writer-prompt.md
- Applied stylistic improvements: consolidated repetitive coordination into list syntax (2 instances)
- Applied systematic hyphenation to ~40+ technical terms (common-noun, error-message, VP-negation, etc.)
- Fixed capitalization: `Markdown-list` → `markdown-list` (common-noun, not proper-noun)
- Added missing determiners to plural/singular common-nouns (8 instances)
- Enhanced prompts with sentence-start rule (v1.0 → v1.1 writer, v1.0 → v1.2 reader)
- Added validation/error-reporting section to reader prompt
- Created knowledge entry 017: sentence-start-rule
- Discussed and rejected "Let-binding" syntax proposal

**Pending:**
- Task A.1.1 (Define semantic check categories) - still awaiting user confirmation for [x]

**Notes:**
- Lint spec now 100% Sloj-conformant with consistent hyphenation
- Prompts enhanced with fundamental sentence structure constraint
- Reader prompt now actively validates and reports Sloj syntax errors
- Identified key rule: "Every Sloj sentence begins with determiner, proper-noun, or keyword"

### 2026-02-05 (Session 2)
**Worked on:**
- Spec refinement (sentence coordination, lexical rules)
- LLM prompts (reader and writer)
- Linter specification
- Comprehensive testing

**Completed:**
- Fixed spec Section 2.1: Added `Either S, or P` and `Neither S, nor P` at sentence level
- Added spec Section 14: Lexical Rules (compound words, multi-word proper nouns, capitalization rules, morphology)
- Created `prompts/sloj-reader-prompt.md` (~2400 tokens, 12 warnings)
- Created `prompts/sloj-writer-prompt.md` (~3500 tokens, 14 rules)
- Created 47 test cases (17 reader + 30 writer)
- Executed all tests: 100% pass rate
- Created `lint/spec/lint-spec.md` (comprehensive linter spec written in Sloj, 42 check types)
- Created 2 evaluation documents in `evaluations/`

**Pending:**
- Task A.1.1 (Define semantic check categories) - completed but not marked [x] in PLAN.md

**Notes:**
- Spec now complete with lexical rules: hyphenation for multi-word entities, proper noun handling, capitalization+determiner rules
- Both LLM prompts are production-ready (v1.0)
- Test results validate prompt effectiveness
- Linter spec defines all semantic checks needed

### 2026-02-05 (Session 1)
**Worked on:**
- Knowledge base initialization
- AGENTS.md and NOTES.md alignment

**Completed:**
- Updated AGENTS.md: `tags` field now only contains terms NOT in filename
- Created 16 knowledge entries (001-016) capturing all persistent design decisions
- Streamlined NOTES.md to focus on session-specific, temporal information

**Notes:**
- Knowledge base initialized with entries covering: project scope, syntax decisions, semantic rules, structural rules, documentary elements, design references
- NOTES.md reduced from 282 to ~90 lines
