# Sloj Linter

A semantic checker for the Sloj Controlled Natural Language for requirements specifications.

## Overview

The Sloj linter (`sloj-lint`) validates Sloj documents for semantic correctness beyond syntax. While a parser checks grammatical validity, the linter ensures semantic consistency, proper usage, and adherence to Sloj best practices.

## Purpose

The linter performs semantic analysis including:

- **Type checking**: Verify noun-verb agreement, determiner-noun compatibility
- **Reference resolution**: Ensure pronouns and references have valid antecedents
- **Scope validation**: Check that modals, negations, and modifiers have proper scope
- **Consistency checks**: Detect contradictions and inconsistencies in requirements
- **Style enforcement**: Ensure adherence to Sloj conventions and best practices
- **Lexicon validation**: Verify that all words are in the Sloj lexicon

## Directory Structure

```
lint/
├── README.md           # This file
├── spec/               # Linter specification and design documents
│   └── src/            # Source specifications
└── src/                # Implementation source code (TBD)
```

## Relationship to Other Tools

The Sloj toolchain consists of:

1. **sloj-check** (syntax checker) - Validates grammar and syntax
2. **sloj-lint** (semantic checker) - **This tool** - validates semantics and style
3. **sloj-parse** - Generates parse trees
4. **sloj-logic** - Maps to formal logic representations

## Status

**Current Status**: Planning phase (Task A.1 in progress)

The linter is currently in the design and specification phase. See the [main project plan](../PLAN.md) for the complete task list (Section A).

## Design Goals

1. **Comprehensive**: Catch semantic errors that pass syntax checking
2. **Helpful**: Provide clear, actionable error messages with suggestions
3. **Configurable**: Allow users to enable/disable specific checks
4. **Fast**: Efficient analysis suitable for real-time editor integration
5. **Extensible**: Easy to add new semantic checks and rules

## Development Plan

See [PLAN.md](../PLAN.md) Section A for the complete development plan.

## Technology Stack

**TBD** - To be determined in task A.2.1.

Potential options:
- **Python** - Rapid development, rich NLP ecosystem
- **Rust** - Performance, type safety, memory safety
- **TypeScript** - Web integration, tooling ecosystem

## Getting Started

This project is in early planning stages. To contribute:

1. Review the [Sloj specification](../spec/sloj-spec-doc.md)
2. Check the [project plan](../PLAN.md) Section A for current tasks
3. See `spec/` directory for design documents (as they are created)

## References

- [Sloj Specification](../spec/sloj-spec-doc.md) - Language definition
- [Sloj Grammar](../spec/sloj-spec-grammar.bnf) - Formal EBNF grammar
- [Sloj Examples](../spec/sloj-examples.md) - Comprehensive examples
- [Project Plan](../PLAN.md) - Overall project roadmap

## License

TBD

---

**Version**: 0.1  
**Status**: Planning  
**Last Updated**: 2026-02-02
