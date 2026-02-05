# Project plan: Sloj

## Objectives
Sloj is an English-based Controlled Natural Language designed for unambiguous software requirements specifications.
It retains readable English structure, but it is deterministically parseable (every sentence has exactly one parse tree) and formally grounded (every parse tree maps to exactly one logical formula).

Sloj text are encoded in UTF-8. 

## Subprojects
- Subprojects are stored in a folder with the same name

## Project-Specific Information
Path: `~/midgard/00_Programming/sloj/`
Authority: `spec/sloj-spec-*.md` 
Artifacts: 
  - `spec/sloj-spec-doc.md` The prose description of the language
  - `spec/sloj-spec-grammar.bnf` The formal EBNF grammar

Tech stack: Markdown specs; tooling TBD
External resources: `ref/sngl-syntax.doc`
Current focus (until changed): Complete the definition of the Sloj language.

## Project Plan
Legend: [ ] todo  [~] doing  [x] done  [!] hold
Rules:
  - set [x] only after explicit user confirmation.
  - the `!` mark for "on hold" applies to all the sub-tasks

1 [x] First draft
- 1.1 [x] write first draft

2 [x] Syntax specification
- 2.1  [x]  grammar disambiguation + examples normalization
- 2.2  [x]  Present perfect (Tier 1)
- 2.3  [x]  Conditionals (Tier 1)
- 2.4  [x]  Passive voice (Tier 1)
- 2.5  [x]  Temporal/future statements (Tier 1)
- 2.6  [x]  Comparisons (Tier 2)
- 2.7  [x]  Measurement phrases (Tier 2)
- 2.8  [x]  Mass comparisons (Tier 2)
- 2.9  [!]  Sentence negation (Tier 2) — deferred, VP negation sufficient for now
- 2.10 [x]  Named references/bindings (Tier 3)
- 2.11 [x]  String literals (Tier 3)
- 2.12 [x]  Annotations (Tier 3)
- 2.13 [x]  Lists/enumeration (Tier 3)
- 2.14 [x]  Nominalizations (Tier 3)
- 2.15 [x]  Purpose clauses (Tier 3)
- 2.16 [x]  Contractions
- 2.17 [x]  Markdown integration specification

3 [!] Formal logic mapping 
- 3.1 [ ] First Order Logic
- 3.2 [ ] Temporal logic
- 3.3 [ ] Operational semantic

4 [ ] Tooling
- 4.1 [!] Grammar checkers
  - 4.1a sloj-check (syntax checker)
  - 4.2a sloj-lint (semantic checker)
- 4.2 [!] Formal logic
  - 4.2a [ ] Ontology builder
  - 4.2b [ ] Model Checker
- 4.3 [x] LLM usage
  - 4.3a [x] sloj-writer prompt
  - 4.3b [x] sloj-reader prompt
- 4.3 [!] Admin tools
  - 4.3a Lexicon Manager
  - 4.3b Ontology Manager

5 [!] Documentation
- 5.1 [ ] Create a Reader's Quick Guide
- 5.2 [ ] Create a Reader's Full Guide
- 5.3 [ ] Create a Writer's FULL Guide
- 5.4 [ ] Reference Manual
- 5.5 [ ] Tutorials

## A. SubProject: lint

Path: `~/midgard/00_Programming/sloj/lint/`
Purpose: Semantic checker for Sloj documents (validates correctness beyond syntax)

A.1 [~] Design & Specification
- A.1.1 [~] Define semantic check categories
- A.1.2 [ ] Design error message format and severity levels
- A.1.3 [ ] Define linter architecture and components
- A.1.4 [ ] Create lexicon schema and initial entries
- A.1.5 [ ] Document configuration format

A.2 [ ] Core Infrastructure
- A.2.1 [ ] Choose technology stack
- A.2.2 [ ] Set up project structure and build system
- A.2.3 [ ] Implement AST representation
- A.2.4 [ ] Implement lexicon loader
- A.2.5 [ ] Create test framework and fixtures

A.3 [ ] Phase 1: Core Semantic Checks
- A.3.1 [ ] Noun-verb agreement (singular/plural)
- A.3.2 [ ] Pronoun resolution and gender agreement
- A.3.3 [ ] Determiner-noun compatibility
- A.3.4 [ ] Modal scope validation
- A.3.5 [ ] VP negation scope validation

A.4 [ ] Phase 2: Reference Validation
- A.4.1 [ ] Antecedent resolution for pronouns
- A.4.2 [ ] Binding validation (proper noun bindings)
- A.4.3 [ ] String literal invisibility to pronouns
- A.4.4 [ ] Relative clause antecedent validation
- A.4.5 [ ] Exception suffix validation

A.5 [ ] Phase 3: Temporal Logic
- A.5.1 [ ] Present perfect usage validation
- A.5.2 [ ] Future tense consistency
- A.5.3 [ ] Temporal sequence validation
- A.5.4 [ ] Conditional tense restrictions
- A.5.5 [ ] Temporal scope validation

A.6 [ ] Phase 4: Advanced Checks
- A.6.1 [ ] Contradiction detection
- A.6.2 [ ] Redundancy detection
- A.6.3 [ ] Completeness checks
- A.6.4 [ ] Ambiguity warnings
- A.6.5 [ ] Partitive validation

A.7 [ ] Phase 5: Style and Best Practices
- A.7.1 [ ] Consistent terminology usage
- A.7.2 [ ] Preferred constructions
- A.7.3 [ ] Readability metrics
- A.7.4 [ ] Documentation completeness
- A.7.5 [ ] Annotation validation

A.8 [ ] Integration & Tooling
- A.8.1 [ ] CLI interface
- A.8.2 [ ] Configuration file support
- A.8.3 [ ] Editor integration (LSP)
- A.8.4 [ ] CI/CD integration
- A.8.5 [ ] Output formats (JSON, text, HTML)

A.9 [ ] Documentation & Release
- A.9.1 [ ] User guide
- A.9.2 [ ] API documentation
- A.9.3 [ ] Check catalog (all rules documented)
- A.9.4 [ ] Example configurations
- A.9.5 [ ] Release v1.0
