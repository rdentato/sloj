# Project Notes: Sloj

**CRITICAL PROTOCOL**: 
1. Always refer to PLAN.md before starting work.
2. **NEVER unblock a task (change `[!]` to `[ ]`) yourself.** Only the user can unblock tasks.
3. You cannot work on blocked phases/steps unless the user explicitly unblocks them.
4. **NEVER mark a task as completed (`[x]`) yourself.** Always ask the user to confirm completion first.

## Current Context

### Core Design Principles
- **Symbolic Coordination**: Replaces natural language `and`/`or` within phrases with explicit operators (`&`, `/`, `&&`, `&&/`), except for fixed VP forms `either...or` and `neither...nor`
- **Explicit Scope**: Uses guillemets `«...»` to strictly delimit scope of modifiers and operators
- **Structural Precedence**: Enforces strict hierarchy of sentence construction
- **Deterministic Parsing**: Every sentence has exactly one parse tree
- **Formal Grounding**: Every parse tree maps to exactly one logical formula

## Key Decisions

- DNF structure at sentence level for clear logical mapping
- Symbolic operators within phrases; natural language at sentence boundaries
- Left-associative, equal-precedence operators to eliminate ambiguity
- `may not` intentionally undefined (use `must not` for prohibition)
- String literals invisible to pronoun resolution
- Code blocks are documentary only; don't participate in syntax
- Nominalizations (event/property nominals) are regular common nouns; no special grammar needed

### Notes
- Spec is at v0.1 Draft status
- Current focus is completing language definition (Phase 2)
- Grammar file exists at `spec/sloj-spec-grammar.bnf`
- **Design Reference**: When uncertain about how to implement a feature, first check `ref/sngl-syntax.doc` to see how it was done in SNGL (Sloj's predecessor)

## Open Questions

- Formal logic mapping (FOL, temporal logic, operational semantics) on hold

### Deferred Features

**Sentence negation (task 2.9)**: Deferred for now. Sloj currently has VP negation (`does not`, `do not`) which covers most use cases. Sentence-level negation (`it is false that S` from Sngl) could be reconsidered in the future if there's a clear need to negate entire propositions, especially complex statements with coordination or quantifiers. For now, VP negation is deemed sufficient.

## Known Issues / Blockers

- Phase 3 (Formal logic mapping) blocked until syntax spec complete
- Phase 4 (Tooling) blocked
- Phase 5 (Documentation) blocked


## Next steps
- Phase 2 (Syntax specification) is **COMPLETE**
- All planned syntax features have been implemented
- Consider next phase: Phase 3 (Formal logic mapping), Phase 4 (Tooling), or Phase 5 (Documentation)
- All subsequent phases are currently blocked and require user decision to unblock


## Session Log

### 2026-02-01
**Worked on:** 
- Rewritten spec document for better balance and conciseness (reduced from ~1740 to ~429 lines)
- Completed spec-grammar consistency evaluation
- Updated grammar section references to match new spec structure
- Clarified that `aka` binding is not in Sloj
- Normalized examples file with consistent formatting
- Added Quick Reference Index (115 entries) with cross-references to spec and grammar
- Added comprehensive Table of Contents to examples file
- Added note in spec document pointing to examples file
- **Completed task 2.1**: Grammar disambiguation + examples normalization
- **Completed task 2.5**: Temporal/future statements (already in spec and grammar)
- All Tier 1 features now complete
- Added VP list negative conjunction form (`NP neither:` + `- nor VP`)
- **Completed task 2.6**: Comparisons (comparative/superlative adjectives and adverbs, equality/inequality, range comparisons)
- Updated `between` construct to use `NounPhrase` instead of `Number and Number` for consistency with symbolic coordination
- **Completed task 2.7**: Measurement phrases (`Number Unit of MassNoun`)
- **Completed task 2.8**: Mass comparisons (`more/less MassNP than MassNP`, `the amount of MassNP`)
- All Tier 2 features now complete (except deferred sentence negation)
- Updated examples file with sections 14 (Measurement Phrases) and 15 (Mass Comparisons)
- Fixed Quick Reference Index and Table of Contents in examples file
- Created evaluation document for measurement/mass additions
- **Completed task 2.15**: Purpose clauses (`in order to` + infinitive VP)
- Added Section 10 to spec document (Purpose Clauses)
- Added Section 29 to examples file with 7 subsections
- Purpose clauses are non-normative (documentary only, don't change formal logic)
- Three forms: prefix, suffix, and incidental
- Created evaluation document for purpose clauses
- **Completed task 2.12**: Annotations (`[[ ... ]]` syntax)
- Added Section 11 to spec document (Annotations)
- Added Section 30 to examples file with 10 subsections
- Annotations are non-normative (metadata only, don't affect formal semantics)
- Support both key-value pairs and free text
- Updated grammar to include Annotation production
- Created evaluation document for annotations
- **Resolved task 2.14**: Nominalizations - decided to treat as regular common nouns
- No special grammar needed; lexicon categorizes event/property nominals
- Added note in spec Section 8.3 (Noun Types)
- **Completed task 2.13**: Lists/enumeration
- Four keywords: `one of:` (exclusive), `any of:` (inclusive), `all of:` (conjunction), `none of:` (exclusion)
- Two forms: inline (comma-separated) and Markdown (bullet list)
- Guillemets required for multiple lists or mid-sentence lists
- Items can be full NPs, numbers, strings, or proper nouns
- Added Section 8.11 to spec document
- Added Section 31 to examples file with 14 subsections
- Updated grammar with InlineList, NPListSent productions

### 2026-02-02 (Session 1)
**Worked on:**
- Discussed and implemented Annotations (`[[ ... ]]` syntax)
- Discussed and resolved Nominalizations (as regular common nouns)
- Discussed and implemented Lists/enumeration (`one of:`, `any of:`, `all of:`, `none of:`)

**Completed:**
- Task 2.12: Annotations - `[[ key: value ]]` and `[[ free text ]]` syntax for metadata
- Task 2.14: Nominalizations - resolved as regular common nouns (no special grammar)
- Task 2.13: Lists/enumeration - four keywords, inline and Markdown forms

**Pending:**
- Phase 2 completion confirmation (all tasks done)
- Phases 3, 4, 5 remain blocked

**Notes:**
- Added design reference note: check `ref/sngl-syntax.doc` when uncertain about implementation
- All Tier 3 features now complete
- Phase 2 (Syntax specification) is functionally complete, pending user confirmation

### 2026-02-02 (Session 2)
**Worked on:**
- Created comprehensive evaluation comparing SNGL vs Sloj (`evaluations/sngl-vs-sloj-comparison-2026-02-02.md`)
- Identified 8 SNGL features not in Sloj; 5 are intentional omissions, 2 were potential additions
- Discussed and implemented open/closed world list distinction using ellipsis syntax

**Completed:**
- SNGL vs Sloj feature comparison evaluation
- Ellipsis syntax for open-world lists (`...` or `…` as final list element)
  - Spec: Added Section 8.11.4 (Open vs Exhaustive Lists), renumbered 8.11.5 (List Rules)
  - Grammar: Added `Ellipsis` and `EllipsisItem` productions
  - Examples: Added Section 31.15 (Open Lists) with 7 subsections
  - Updated Quick Reference Index and Table of Contents

**Pending:**
- Interpretation Rules appendix (documentation improvement, deferred to future session)
- Phase 2 completion confirmation
- Phases 3, 4, 5 remain blocked

**Key Decisions:**
- Lists are **exhaustive by default** (closed-world) — safer assumption for requirements
- Trailing `...` or `…` indicates **open list** (extensible)
- Works with all list keywords and both inline/Markdown formats
- Rejected alternatives: `only one of:` (changes cardinality meaning), `any only among:` (unnatural)

**SNGL Features Intentionally Omitted:**
- Elverson pronouns (`yey/yem`) — `self-` prefix handles reflexive
- Sentence negation (`it is false that`) — VP negation sufficient (task 2.9 deferred)
- VP precedence hierarchy — equal precedence is simpler
- Triple-delimiter strings (`⸄⸅`) — two delimiters sufficient
- `aka` binding — redundant with `named`/`called`

### 2026-02-02 (Session 3)
**Worked on:**
- Added contractions to Sloj language
- Marked Phase 2 as complete
- Initialized lint subproject
- Created Sloj-Markdown integration specification

**Completed:**
- Contractions feature (resolves open question)
  - Spec: Added Section 3.6.1 (Contractions) with full table and rules
  - Grammar: Added contracted forms to Modal, VPNeg, FutureAux, and PerfectNeg productions
  - Examples: Added Section 6.1 (Contractions) with 10 examples
  - Updated Quick Reference Index and Table of Contents
- **Phase 2 (Syntax specification) - COMPLETE**
- Lint subproject initialization
  - Created `lint/README.md` with project overview
  - Added Section A to PLAN.md with 9-phase development plan (A.1-A.9)
  - Created directory structure: `lint/spec/`, `lint/src/`
- **Task 2.17: Markdown integration specification - COMPLETE**
  - Created evaluation document (`evaluations/sloj-markdown-integration-2026-02-02.md`)
  - Defined three-layer model: Markdown structure, Sloj sentences, integration points
  - Identified integration points: Markdown lists (VP/NP/conditional/while), code blocks, annotations
  - Specified linter implications: two-phase parsing, check categories
  - Addressed open questions: inline code, nested lists, ordered lists, indentation, blank lines
  - Created `spec/sloj-spec-markdown.md` (complete specification, 14 sections + appendix)
  - Updated `spec/sloj-spec-doc.md` with Section 13 (Document Structure and Markdown Integration)
  - Updated `spec/sloj-spec-grammar.bnf` with Markdown integration comment
  - Defined Sloj text vs free text rules
  - Specified tables as free text (all content)
  - Documented two-phase parsing model
  - Provided validation and linting guidelines

**Key Decisions:**
- Contractions are **lexical alternatives** to full forms (semantically equivalent)
- Both forms may be used interchangeably within a document
- Supported contractions: `isn't`, `aren't`, `doesn't`, `don't`, `hasn't`, `haven't`, `won't`, `mustn't`, `shouldn't`, `can't`
- `can't` is an alternative to `is not able to` / `are not able to`
- Contractions follow the same scope and attachment rules as their full forms
- **Markdown integration**: Three-layer model with clear boundaries between Sloj syntax and Markdown formatting
- **Markdown lists are part of Sloj grammar** (VP lists, NP lists, conditional lists, while lists)
- **Code blocks and annotations** are documentary elements at sentence level
- **All other Markdown** (headings, emphasis, links, etc.) is pure formatting and ignored by parser

**Pending:**
- Task A.1.1: Define semantic check categories (in progress)
- Task A.1.2: Design error message format and severity levels
- Task A.1.3: Define linter architecture and components

**Notes:**
- Phase 2 included all Tier 1, 2, and 3 features plus contractions
- Task 2.9 (Sentence negation) remains deferred - VP negation is sufficient for current needs
- Sloj v0.1 syntax specification is now complete and stable
- Started Phase 4 (Tooling) with lint subproject





---

## Session Summary (2026-02-02, Session 3)

**Duration**: Full session  
**Focus**: Contractions, Phase 2 completion, Markdown integration specification, Lint subproject initialization

### Deliverables

**Specifications Created/Updated**:
- `spec/sloj-spec-markdown.md` (NEW, 675 lines) - Complete Markdown integration specification
- `spec/sloj-spec-doc.md` (UPDATED, 819 lines) - Added Section 13 on Markdown integration
- `spec/sloj-spec-grammar.bnf` (UPDATED, 565 lines) - Added Markdown integration comment
- `spec/sloj-examples.md` (UPDATED, 2,193 lines) - Added Section 6.1 on contractions

**Evaluations**:
- `evaluations/sloj-markdown-integration-2026-02-02.md` - Comprehensive design evaluation

**Subprojects**:
- `lint/README.md` - Linter project overview
- `lint/spec/` - Specification directory created
- `lint/src/` - Source directory created

### Tasks Completed

**Phase 2 (Syntax Specification)**:
- Task 2.16: Contractions ✅
- Task 2.17: Markdown integration specification ✅
- **Phase 2 marked COMPLETE** ✅

**SubProject A (lint)**:
- Initialized project structure
- Created 9-phase development plan (45 tasks)
- Started A.1.1: Define semantic check categories

### Key Decisions Made

**Contractions**:
- Added 10 contracted forms as lexical alternatives
- Forms: isn't, aren't, doesn't, don't, hasn't, haven't, won't, mustn't, shouldn't, can't
- Semantically equivalent to full forms
- Interchangeable within documents

**Markdown Integration**:
- Three-layer model: Markdown structure / Sloj sentences / Integration points
- Sloj text: Paragraph sentences, list items (introduced by :), annotation content
- Free text: Headings, tables (ALL content), blockquotes, links, images, URLs
- Integration points: VP/NP/conditional/while lists, code blocks, annotations
- Forbidden in Sloj text: Emphasis, links, inline code, images
- Two-phase parsing: Markdown extraction → Sloj parsing

### Statistics

**Total Specification Lines**: 5,446 lines
- Core spec: 819 lines
- Grammar: 565 lines
- Markdown spec: 675 lines
- Examples: 2,193 lines

**Phase 2 Status**: 17 tasks complete, 1 deferred (sentence negation)

### Recommended Next Steps

1. Continue A.1.1: Define semantic check categories for linter
2. Complete A.1.2-A.1.5: Finish linter design phase
3. Consider unblocking Phase 3 (Formal logic mapping) or Phase 5 (Documentation)

