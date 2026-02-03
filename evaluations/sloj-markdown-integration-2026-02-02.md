# Sloj-in-Markdown Integration Evaluation

**Date**: 2026-02-02  
**Status**: Design Evaluation  
**Purpose**: Define how Sloj interacts with Markdown document structure

---

## 1. Executive Summary

This document evaluates and defines how Sloj (a controlled natural language for requirements) integrates with Markdown document structure. Sloj requirements are embedded within Markdown documents, allowing for rich documentation alongside formal specifications.

**Key Findings:**
- Sloj sentences are the primary content; Markdown provides document structure
- Clear separation between Sloj syntax and Markdown formatting
- Code blocks and annotations are the main integration points
- Markdown lists are structural elements in Sloj grammar (VP lists, NP lists)

---

## 2. Background

### 2.1 Sloj Document Model

From the grammar (`sloj-spec-grammar.bnf`):

```ebnf
Document          := DocumentElement+
DocumentElement   := Annotation* (Sentence | CodeBlock)
```

This defines a **flat sequence** of:
1. **Sentences** - Sloj requirements (optionally preceded by annotations)
2. **Code blocks** - Documentary elements (fenced with ``` or ~~~)

### 2.2 SNGL Precedent

SNGL (Sloj's predecessor) references a separate specification: `sngl-spec-markdown.md` (not available in our reference). However, we can infer from `sngl-syntax.md`:

- Markdown lists are used for subject-scoped VP coordination
- The `:` character at end-of-line introduces a Markdown list
- Code blocks are documentary elements

---

## 3. Integration Points

### 3.1 Markdown Lists as Sloj Syntax

Markdown lists are **part of Sloj syntax**, not just formatting:

#### 3.1.1 VP Lists (Verb Phrase Coordination)

**Syntax**: `NP:` followed by Markdown list

```sloj
The system:
  - validates the input.
  - logs the result.
  - sends a notification.
```

**Grammar Production**:
```ebnf
VPList := 'either'? ':' NEWLINE VPListItems
VPListItems := VPListItem+
VPListItem := '-' 'or'? 'then'? 'nor'? VerbPhrase '.'
```

**Rules**:
- `:` must be the last non-whitespace character on the line
- Next non-blank line must start a Markdown list (`-` prefix)
- Each list item ends with `.`
- VP lists must be the final element of a sentence

#### 3.1.2 NP Lists (Noun Phrase Enumeration)

**Syntax**: `NP VP ListKeyword:` followed by Markdown list

```sloj
The value is one of:
- 3.
- "pippo".
- xld.
```

**Grammar Production**:
```ebnf
NPListSent := NounPhrase VPHead ListKeyword ':' NEWLINE NPListItems EllipsisItem?
NPListItems := NPListItem+
NPListItem := '-' ListItem '.'
```

**Rules**:
- List keywords: `one of`, `any of`, `all of`, `none of`
- Each item ends with `.`
- Optional ellipsis (`- ...`) as final item indicates open list
- NP lists must be at end of sentence

#### 3.1.3 Conditional Lists

**Syntax**: Conditional with `:` followed by Markdown list

```sloj
If the user is authenticated:
  - The system grants access.
  - The session is created.
```

**Grammar Production**:
```ebnf
ConditionalSentWithList := CondKeyword ConditionClause 'either'? ':' NEWLINE ConsequenceList
ConsequenceList := ConsequenceListItem+
ConsequenceListItem := '-' 'or'? 'then'? NounPhrase ConsequenceBody '.'
```

#### 3.1.4 While Lists

**Syntax**: `while` condition with `:` followed by Markdown list

```sloj
While the system is active:
  - The monitor checks status.
  - The logger records events.
```

**Grammar Production**:
```ebnf
WhileSentWithList := 'while' ConditionClause ', either'? ':' NEWLINE ConsequenceList
```

### 3.2 Fenced Code Blocks

Code blocks are **documentary elements** that appear at sentence level.

**Syntax**:
```
```language
code content
```
```

**Grammar Production**:
```ebnf
CodeBlock := TripleBacktickBlock | TripleTildeBlock
TripleBacktickBlock := '```' SP* Language? NEWLINE CodeContent NEWLINE '```'
TripleTildeBlock := '~~~' SP* Language? NEWLINE CodeContent NEWLINE '~~~'
```

**Rules**:
- Code blocks appear **between sentences**, not inline
- Invisible to pronoun resolution
- Do not participate in VP/NP coordination, relative clauses, or conditionals
- Purely documentary (not part of formal semantics)
- Optional language identifier after opening fence

**Examples**:
```sloj
The API must accept JSON requests.

```json
{
  "user": "alice",
  "action": "login"
}
```

The system validates the request.
```

### 3.3 Annotations

Annotations provide metadata and appear on their own line(s) before sentences or code blocks.

**Syntax**: `[[ content ]]`

**Grammar Production**:
```ebnf
Annotation := '[[' AnnotationContent ']]' NEWLINE
AnnotationContent := KeyValuePairs | FreeText
KeyValuePairs := KeyValuePair (',' SP* KeyValuePair)*
KeyValuePair := Identifier ':' SP* AnnotationValue
```

**Rules**:
- Appear on own line(s) immediately before the element they annotate
- Non-normative (metadata only)
- Do not affect formal semantics

**Example**:
```sloj
[[ id: REQ-001, priority: high ]]
The system must respond within 100 milliseconds.
```

---

## 4. Markdown Elements NOT Part of Sloj Syntax

The following Markdown elements are **formatting only** and not part of Sloj grammar:

### 4.1 Headings

```markdown
# Section Title
## Subsection
### Sub-subsection
```

**Status**: Pure Markdown formatting
**Purpose**: Document organization
**Interaction**: None - headings are ignored by Sloj parser

### 4.2 Paragraphs

Blank lines separate paragraphs in Markdown but have no semantic meaning in Sloj.

**Rule**: Sloj sentences are delimited by `.` (period), not by paragraph breaks.

### 4.3 Emphasis and Formatting

```markdown
*italic*, **bold**, `inline code`, ~~strikethrough~~
```

**Status**: Not allowed within Sloj sentences
**Rationale**: Sloj is plain text; formatting would introduce ambiguity

### 4.4 Links and Images

```markdown
[link text](url)
![alt text](image.png)
```

**Status**: Not allowed within Sloj sentences
**Rationale**: Use string literals for URLs; images are documentary

### 4.5 Blockquotes

```markdown
> quoted text
```

**Status**: Not part of Sloj
**Usage**: May be used for non-normative commentary

### 4.6 Horizontal Rules

```markdown
---
***
```

**Status**: Pure Markdown formatting
**Purpose**: Visual separation

### 4.7 Tables

```markdown
| Column 1 | Column 2 |
|----------|----------|
| Data     | Data     |
```

**Status**: Not part of Sloj syntax
**Usage**: May be used for documentary content

### 4.8 HTML

**Status**: Not allowed
**Rationale**: Sloj is plain text; HTML introduces complexity

---

## 5. Document Structure Model

### 5.1 Conceptual Layers

A Sloj-in-Markdown document has three conceptual layers:

```
┌─────────────────────────────────────────┐
│  Layer 3: Markdown Document Structure  │  ← Headings, sections, formatting
│  (organizational, non-normative)        │
├─────────────────────────────────────────┤
│  Layer 2: Sloj Sentences                │  ← Requirements (normative)
│  (formal, parseable, semantic)          │
├─────────────────────────────────────────┤
│  Layer 1: Sloj-Markdown Integration     │  ← Lists, code blocks, annotations
│  (structural, part of grammar)          │
└─────────────────────────────────────────┘
```

**Layer 1** (Integration): Markdown constructs that are part of Sloj grammar
- VP lists, NP lists, conditional lists, while lists
- Fenced code blocks
- Annotations

**Layer 2** (Sloj Core): Pure Sloj sentences
- Subject-verb-object structures
- Coordination operators (`&`, `/`, `&&`, etc.)
- Modals, negations, temporal statements
- All formal requirements

**Layer 3** (Markdown Wrapper): Pure Markdown formatting
- Headings, paragraphs, emphasis
- Links, images, tables
- Blockquotes, horizontal rules

### 5.2 Parsing Model

**Two-phase parsing**:

1. **Markdown Pre-processing**:
   - Extract Sloj content (sentences, lists, code blocks, annotations)
   - Preserve list structure and code block boundaries
   - Discard pure Markdown formatting (headings, emphasis, etc.)

2. **Sloj Parsing**:
   - Parse extracted Sloj sentences
   - Integrate Markdown lists as syntactic elements
   - Associate annotations with following elements
   - Treat code blocks as opaque documentary content

### 5.3 Example Document

```markdown
# User Authentication Requirements

This section specifies authentication requirements.

[[ id: REQ-AUTH-001, priority: critical ]]
The system must authenticate users.

## Login Process

When a user logs in, the system:
  - validates the credentials.
  - creates a session.
  - logs the event.

The session must expire after 30 minutes.

## Password Requirements

[[ id: REQ-AUTH-002 ]]
A password must satisfy all of:
- at least 8 characters.
- at least one digit.
- at least one special character.
- ...

Example of a valid password:

```
P@ssw0rd123
```

The system must not store plaintext passwords.
```

**Parsing result**:

- **Markdown structure**: 2 headings, 2 paragraphs of commentary
- **Sloj sentences**: 4 requirements
- **Annotations**: 2 metadata blocks
- **VP list**: 1 (login process)
- **NP list**: 1 (password requirements, open list)
- **Code block**: 1 (example password)

---

## 6. Ambiguity Resolution

### 6.1 Colon (`:`) Disambiguation

The `:` character has multiple uses:

1. **Sloj list introducer** (end of line, followed by Markdown list)
2. **Annotation key-value separator** (within `[[ ]]`)
3. **List keyword component** (`one of:`, `any of:`, etc.)

**Resolution**:
- Context-dependent parsing
- End-of-line `:` followed by Markdown list → Sloj list syntax
- `:` within `[[ ]]` → annotation syntax
- `:` after list keyword → inline or Markdown list

### 6.2 Hyphen (`-`) Disambiguation

The `-` character has multiple uses:

1. **Markdown list item marker**
2. **Hyphen in compound words** (`self-authenticate`, `multi-factor`)
3. **Minus sign** (in numbers: `-5`)

**Resolution**:
- At start of line (after whitespace) → Markdown list item
- Within word → hyphen
- Before digit → minus sign

### 6.3 Period (`.`) Disambiguation

The `.` character has multiple uses:

1. **Sentence terminator**
2. **List item terminator**
3. **Decimal point** (in numbers: `3.14`)
4. **Ellipsis component** (`...`)

**Resolution**:
- After complete sentence → sentence terminator
- At end of list item → list item terminator
- Between digits → decimal point
- Three consecutive → ellipsis

---

## 7. Linter Implications

### 7.1 Document Structure Validation

The linter must validate:

1. **List structure**:
   - `:` at end of line is followed by Markdown list
   - List items have correct markers (`-`, `or`, `then`, `nor`)
   - List items end with `.`
   - VP/NP lists are final elements

2. **Code block structure**:
   - Properly fenced (matching ``` or ~~~)
   - Appear at sentence level (not mid-sentence)
   - Optional language identifier is valid

3. **Annotation structure**:
   - Properly delimited (`[[ ]]`)
   - On own line(s)
   - Immediately before annotated element
   - Valid key-value syntax or free text

### 7.2 Markdown-Sloj Boundary Validation

The linter should warn about:

1. **Markdown formatting in Sloj sentences**:
   - `*italic*`, `**bold**` within sentences
   - `[links](url)` within sentences
   - Inline `code` within sentences (use backtick strings instead)

2. **Incomplete integration**:
   - `:` at end of line not followed by list
   - Markdown list not preceded by `:` introducer
   - List items without proper terminators

3. **Ambiguous constructs**:
   - Unclear whether content is Sloj or Markdown commentary
   - Mixed formatting styles

### 7.3 Extraction and Validation Workflow

```
┌─────────────────────┐
│  Markdown Document  │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Extract Sloj       │  ← Identify sentences, lists, code blocks, annotations
│  Content            │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Validate Markdown  │  ← Check list structure, code blocks, annotations
│  Integration        │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Parse Sloj         │  ← Full Sloj grammar parsing
│  Sentences          │
└──────────┬──────────┘
           │
           ▼
┌─────────────────────┐
│  Semantic           │  ← Agreement, references, scope, etc.
│  Validation         │
└─────────────────────┘
```

---

## 8. Recommendations

### 8.1 Specification Updates

1. **Create `sloj-spec-markdown.md`**:
   - Dedicated specification for Sloj-in-Markdown integration
   - Detailed rules for list syntax, code blocks, annotations
   - Examples of valid and invalid constructs

2. **Update `sloj-spec-doc.md`**:
   - Add section on document structure
   - Clarify Markdown integration points
   - Reference the dedicated Markdown spec

3. **Update grammar comments**:
   - Add notes about Markdown list syntax
   - Clarify NEWLINE handling in lists
   - Document code block and annotation placement rules

### 8.2 Linter Design

1. **Two-phase parsing**:
   - Phase 1: Markdown structure extraction
   - Phase 2: Sloj syntax and semantics

2. **Separate check categories**:
   - `markdown-structure`: List syntax, code blocks, annotations
   - `markdown-boundary`: Formatting in Sloj sentences
   - `sloj-syntax`: Core Sloj grammar
   - `sloj-semantics`: Agreement, references, scope

3. **Clear error messages**:
   - Distinguish Markdown vs Sloj errors
   - Provide context (line numbers, surrounding text)
   - Suggest fixes (e.g., "Did you mean to start a list? Add `:` at end of line")

### 8.3 Tooling Considerations

1. **Editor integration**:
   - Syntax highlighting for Sloj within Markdown
   - Distinguish Sloj sentences from Markdown commentary
   - Highlight integration points (lists, code blocks, annotations)

2. **Conversion tools**:
   - Extract pure Sloj from Markdown (strip formatting)
   - Convert Sloj to Markdown (add structure)
   - Validate round-trip conversion

3. **Documentation generation**:
   - Render Sloj-in-Markdown to HTML/PDF
   - Preserve both Sloj semantics and Markdown structure
   - Generate formal logic alongside readable documentation

---

## 9. Open Questions

### 9.1 Inline Code vs Backtick Strings

**Question**: Should inline Markdown code (`` `code` ``) be allowed in Sloj sentences?

**Current Status**: Sloj uses backtick strings (`` `string` ``) for string literals.

**Options**:
1. **Allow**: Treat inline code as backtick strings
2. **Forbid**: Require explicit string literal syntax
3. **Distinguish**: Different semantics for inline code vs strings

**Recommendation**: **Forbid** - Keep Sloj syntax explicit and unambiguous. Use backtick strings for literals.

### 9.2 Nested Lists

**Question**: Should Sloj support nested Markdown lists?

**Current Status**: Grammar does not support nested lists.

**Example**:
```markdown
The system:
  - validates input:
    - checks format.
    - checks length.
  - logs result.
```

**Recommendation**: **Forbid** - Nested lists introduce complexity. Use separate sentences or guillemets for grouping.

### 9.3 Ordered Lists

**Question**: Should Sloj support ordered Markdown lists (`1.`, `2.`, etc.)?

**Current Status**: Grammar uses unordered lists (`-`) only.

**Recommendation**: **Forbid** - Unordered lists are sufficient. Ordering is expressed via `then` markers, not list numbering.

### 9.4 Indentation Levels

**Question**: How much indentation is required/allowed for list items?

**Current Status**: Not specified in grammar.

**Recommendation**: **Flexible** - Allow 2-4 spaces or 1 tab. Linter should warn about inconsistent indentation within a document.

### 9.5 Blank Lines in Lists

**Question**: Are blank lines allowed between list items?

**Current Status**: Not specified.

**Recommendation**: **Allow** - Blank lines improve readability for long list items. Parser should skip blank lines within lists.

---

## 10. Conclusion

Sloj integrates with Markdown at specific, well-defined points:

1. **Markdown lists** are part of Sloj syntax (VP lists, NP lists, conditional lists)
2. **Code blocks** are documentary elements at sentence level
3. **Annotations** provide metadata on their own lines
4. **All other Markdown** is pure formatting (headings, emphasis, links, etc.)

This integration provides:
- **Formal requirements** (Sloj sentences) with precise semantics
- **Rich documentation** (Markdown structure) for human readers
- **Clear boundaries** between normative and documentary content

The linter must validate both Sloj syntax and Markdown integration, ensuring correct usage of lists, code blocks, and annotations while warning about ambiguous or incorrect constructs.

**Next Steps**:
1. Create `sloj-spec-markdown.md` specification
2. Implement Markdown extraction in linter (Task A.2.3)
3. Add Markdown-specific validation checks (Task A.3)

---

**Document Status**: Complete  
**Reviewed**: Pending  
**Approved**: Pending
