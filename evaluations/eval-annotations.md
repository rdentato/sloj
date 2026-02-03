# Evaluation: Annotations Feature

**Date**: 2026-02-01  
**Feature**: Annotations (Task 2.12)  
**Status**: Completed

---

## Overview

This evaluation documents the addition of annotations to Sloj using the `[[ ... ]]` syntax. Annotations provide metadata for requirements without affecting their formal semantics.

---

## Design Decisions

### 1. Syntax Choice: `[[ ... ]]`

**Rationale:**
- Visually distinct and easy to spot
- Symmetric and balanced delimiters
- Familiar from other contexts (wiki links, shell arrays)
- Does not conflict with existing Sloj syntax
- More readable than HTML comments `<!-- ... -->`

**Alternatives considered:**
- `@key:value` (inline prefix) - Less flexible for free text
- `@[ ... ]` (suffix) - Could interfere with readability
- Markdown comments `<!-- ... -->` - Too verbose and ugly

### 2. Placement Rules

**Decision:** Annotations appear on their own line(s) immediately before the element they annotate.

**Rationale:**
- Clear association with following element
- No ambiguity about what is being annotated
- Consistent with purpose clauses (which also precede)
- Easy to parse and extract

### 3. Content Format

**Decision:** Support both key-value pairs and free text.

**Formats:**
- Key-value: `[[ key: value, key2: value2 ]]`
- Free text: `[[ This is a note about the requirement. ]]`

**Rationale:**
- Key-value pairs for structured metadata (IDs, priorities, links)
- Free text for rationale, context, and human-readable notes
- Flexibility for different use cases without overcomplicating syntax

### 4. Non-Normative Status

**Decision:** Annotations are completely non-normative and do not affect formal semantics.

**Rationale:**
- Consistent with code blocks and purpose clauses
- Keeps the core language focused on requirements
- Metadata belongs to requirements management, not the language itself
- Tools can extract and use annotations without changing requirement meaning

---

## Grammar Changes

### Document Structure

**Before:**
```ebnf
DocumentElement := Sentence | CodeBlock
```

**After:**
```ebnf
DocumentElement := Annotation* (Sentence | CodeBlock)
```

### New Productions

```ebnf
Annotation        := '[[' AnnotationContent ']]' NEWLINE

AnnotationContent := KeyValuePairs | FreeText

KeyValuePairs     := KeyValuePair (',' SP* KeyValuePair)*

KeyValuePair      := Identifier ':' SP* AnnotationValue

AnnotationValue   := <annotation_value>

FreeText          := <free_text>

Identifier        := <identifier>

NEWLINE           := '\n'
```

---

## Specification Changes

### New Section 11: Annotations

Added comprehensive section covering:
- Syntax (key-value pairs, free text, multiple lines)
- Rules (placement, non-normative, content, no nesting)
- Common use cases (requirements management, traceability, rationale, versioning)
- Semantics (documentary elements, metadata only)
- Multiple annotations
- Non-normative nature

### Section Renumbering

- Old Section 11 "Explicit Grouping (Guillemets)" → New Section 12

---

## Examples Added

Added Section 30 to examples file with 10 subsections:

1. **Basic Annotations (Key-Value Pairs)** - Simple metadata
2. **Multiple Annotations** - Multiple lines of annotations
3. **Free Text Annotations** - Rationale and context
4. **Traceability Annotations** - Links to external systems
5. **Versioning and Status Annotations** - Workflow metadata
6. **Annotations with Code Blocks** - Annotating code examples
7. **Annotations with Conditionals** - Annotating conditional statements
8. **Annotations with Temporal Statements** - Annotating temporal requirements
9. **Mixed Annotations** - Combining key-value and free text
10. **Non-Normative Nature** - Clarifying documentary status

Total examples: 25+ distinct annotation patterns

---

## Quick Reference Index Updates

Added 7 new entries:
- Annotation key-value
- Annotation multiple
- Annotation free text
- Annotation traceability
- Annotation versioning
- Annotation with code
- Annotation non-normative

---

## Use Cases Supported

### 1. Requirements Management
```sloj
[[ id: REQ-001, priority: high, author: alice ]]
The system must respond within 100 milliseconds.
```

### 2. Traceability
```sloj
[[ links: JIRA-123, TEST-456 ]]
The API must support OAuth 2.0 authentication.
```

### 3. Rationale/Context
```sloj
[[ This requirement addresses the performance issue reported in Q3. ]]
The cache must be cleared every 5 minutes.
```

### 4. Versioning
```sloj
[[ version: 2.1, date: 2024-01-15, status: approved ]]
The API must support OAuth 2.0 authentication.
```

### 5. Compliance
```sloj
[[ compliance: ISO-27001, section: 5.2.3 ]]
The system must encrypt all data at rest & in transit.
```

---

## Consistency with Sloj Principles

### ✅ Deterministic Parsing
- Annotations have clear delimiters and placement rules
- No ambiguity about what is being annotated
- Parser can easily identify and extract annotations

### ✅ Formal Grounding
- Annotations are explicitly non-normative
- They do not affect the logical formula mapping
- Requirement semantics remain unchanged

### ✅ Readability
- `[[ ... ]]` syntax is clean and unobtrusive
- More readable than HTML comments
- Clear visual separation from requirements

### ✅ Consistency with Existing Features
- Similar to purpose clauses (non-normative, documentary)
- Similar to code blocks (invisible to semantics)
- Follows pattern of keeping metadata separate from logic

---

## Potential Tool Support

### Parser/Validator
- Extract annotations during parsing
- Validate key-value format
- Check for required metadata fields

### Requirements Management
- Track requirement IDs and versions
- Link to external systems (JIRA, test cases)
- Manage approval workflow

### Documentation Generator
- Include annotations in generated docs
- Create traceability matrices
- Generate requirement catalogs

### Linter
- Warn about missing required annotations (e.g., `id`)
- Check for deprecated requirements
- Validate metadata format

---

## Open Questions

None. The feature is well-defined and consistent with Sloj's design principles.

---

## Conclusion

Annotations provide a clean, flexible way to add metadata to Sloj requirements without affecting their formal semantics. The `[[ ... ]]` syntax is readable, unambiguous, and consistent with Sloj's design principles.

**Status**: ✅ Complete and ready for use

**Task 2.12**: [x] Annotations (Tier 3) - Pending user confirmation
