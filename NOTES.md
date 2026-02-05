# Project Notes: Sloj

**CRITICAL PROTOCOL**: 
1. Always refer to PLAN.md before starting work.
2. **NEVER unblock a task (change `[!]` to `[ ]`) yourself.** Only the user can unblock tasks.
3. You cannot work on blocked phases/steps unless the user explicitly unblocks them.
4. **NEVER mark a task as completed (`[x]`) yourself.** Always ask the user to confirm completion first.

## Current Context

- Spec is at v0.1 Draft status
- Phase 2 (Syntax specification) is **COMPLETE**
- Phase 4 (Tooling) started with lint subproject
- Grammar file: `spec/sloj-spec-grammar.bnf`
- Persistent knowledge stored in `knowledge/` (16 entries)

## Open Questions

- Formal logic mapping (FOL, temporal logic, operational semantics) on hold

## Deferred Features

- **Sentence negation (task 2.9)**: VP negation (`does not`, `do not`) covers most use cases. Sentence-level negation (`it is false that S`) deferred.

## Known Issues / Blockers

- Phase 3 (Formal logic mapping) blocked until syntax spec complete
- Phase 4 (Tooling) partially unblocked (lint subproject)
- Phase 5 (Documentation) blocked

## Session Log

### 2026-02-01
**Worked on:** 
- Rewritten spec document (reduced from ~1740 to ~429 lines)
- Completed spec-grammar consistency evaluation
- Added Quick Reference Index (115 entries)
- Completed tasks 2.1, 2.5, 2.6, 2.7, 2.8, 2.12, 2.13, 2.14, 2.15

**Completed:**
- All Tier 1, Tier 2 features (except deferred sentence negation)
- All Tier 3 features

### 2026-02-02 (Session 1)
**Worked on:**
- Annotations, Nominalizations, Lists/enumeration

**Completed:**
- Task 2.12: Annotations
- Task 2.14: Nominalizations (resolved as regular common nouns)
- Task 2.13: Lists/enumeration

### 2026-02-02 (Session 2)
**Worked on:**
- SNGL vs Sloj comparison evaluation
- Open/closed world list distinction

**Completed:**
- Ellipsis syntax for open-world lists

**Pending:**
- Interpretation Rules appendix (deferred)

### 2026-02-02 (Session 3)
**Worked on:**
- Contractions, Phase 2 completion, Markdown integration, Lint subproject init

**Completed:**
- Task 2.16: Contractions
- Task 2.17: Markdown integration specification
- **Phase 2 (Syntax specification) - COMPLETE**
- Lint subproject initialization

**Pending:**
- Task A.1.1: Define semantic check categories
- Task A.1.2: Design error message format and severity levels
- Task A.1.3: Define linter architecture and components

### 2026-02-05
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
