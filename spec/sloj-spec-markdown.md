# Sloj-in-Markdown Specification

**Version**: 0.1  
**Status**: Draft  
**Date**: 2026-02-02

---

## 1. Overview

This document specifies how Sloj (a controlled natural language for requirements) integrates with Markdown document structure. Sloj requirements are embedded within Markdown documents, allowing for rich documentation alongside formal specifications.

**Key Principle**: Sloj sentences are the normative content; Markdown provides document structure and formatting.

---

## 2. Document Model

### 2.1 Three-Layer Architecture

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
- Coordination operators (\`&\`, \`/\`, \`&&\`, etc.)
- Modals, negations, temporal statements
- All formal requirements

**Layer 3** (Markdown Wrapper): Pure Markdown formatting
- Headings, paragraphs, emphasis
- Links, images, tables
- Blockquotes, horizontal rules

### 2.2 Grammar Foundation

From \`sloj-spec-grammar.bnf\`:

```ebnf
Document          := DocumentElement+
DocumentElement   := Annotation* (Sentence | CodeBlock)
```

This defines a flat sequence of Sloj sentences, code blocks, and annotations, embedded within Markdown structure.

---

## 3. Sloj Text vs Free Text

### 3.1 Sloj Text (Parsed and Validated)

The following content is parsed as Sloj and must conform to Sloj grammar:

1. **Paragraph text**: Regular prose paragraphs outside of special Markdown constructs
2. **List items**: When part of Sloj list syntax (VP lists, NP lists, conditional lists, while lists)
3. **Annotation content**: Text within \`[[ ]]\` delimiters

**Example**:
```markdown
The system must validate input.

The user can export data.
```

Both sentences are Sloj text and will be parsed and validated.

### 3.2 Free Text (Not Parsed as Sloj)

The following content is free text and NOT parsed as Sloj:

1. **Headings**: All heading levels (\`#\`, \`##\`, \`###\`, etc.)
2. **Table headers**: First row of Markdown tables
3. **Table cells**: All table cell content
4. **URLs**: Bare URLs or within link syntax
5. **Link text**: Text in \`[link text](url)\`
6. **Image alt text**: Text in \`![alt text](image.png)\`
7. **Blockquotes**: Content within \`> quote\` syntax
8. **Inline code**: Content in backticks (unless part of Sloj string literal in paragraph)
9. **HTML comments**: \`<!-- comment -->\`
10. **Horizontal rules**: \`---\`, \`***\`, \`___\`

**Example**:
```markdown
# User Authentication Requirements

This section describes authentication.

| ID | Description | Priority |
|----|-------------|----------|
| REQ-001 | Login feature | High |

The system must authenticate users.
```

- Heading: Free text
- Paragraph "This section...": Free text (documentary)
- Table: All content is free text
- Sentence "The system...": Sloj text (requirement)

### 3.3 Distinguishing Sloj Text from Free Text

**Rule**: Paragraph text is Sloj text if it forms complete Sloj sentences (ends with \`.\ and has proper structure).

**Documentary paragraphs** (free text):
- Introductory text
- Explanatory notes
- Section descriptions
- Commentary

**Requirement paragraphs** (Sloj text):
- Formal requirements
- Specifications
- Constraints
- Rules

**Best Practice**: Use clear visual separation (blank lines, headings) between documentary text and requirements.

---

## 4. Markdown Lists as Sloj Syntax

Markdown lists are **part of Sloj grammar** when introduced by specific Sloj syntax.

### 4.1 VP Lists (Verb Phrase Coordination)

**Syntax**: \`NounPhrase :\` at end of line, followed by Markdown list

**Grammar**:
```ebnf
VPList := 'either'? ':' NEWLINE VPListItems
        | 'neither' ':' NEWLINE VPListItems
VPListItems := VPListItem+
VPListItem := '-' 'or'? 'then'? 'nor'? VerbPhrase '.'
```

**Forms**:

1. **Parallel conjunction** (default):
```sloj
The system:
  - validates the input.
  - logs the result.
  - sends a notification.
```

2. **Sequential conjunction**:
```sloj
The player:
  - moves three steps.
  - then pushes the button.
  - then opens the bottle.
```

3. **Exclusive disjunction**:
```sloj
The light either:
  - blinks.
  - or turns off.
  - or becomes green.
```

4. **Inclusive disjunction**:
```sloj
The system:
  - or validates locally.
  - or validates remotely.
```

5. **Negative conjunction**:
```sloj
The system neither:
  - nor stores passwords.
  - nor logs credentials.
```

**Rules**:
- \`:\` MUST be the last non-whitespace character on the line
- Next non-blank line MUST start a Markdown list (\`-\` prefix)
- Each list item MUST end with \`.\`
- VP lists MUST be the final element of a sentence
- Indentation: 2-4 spaces or 1 tab (consistent within document)
- Blank lines between items are allowed

### 4.2 NP Lists (Noun Phrase Enumeration)

**Syntax**: \`NounPhrase VerbPhrase ListKeyword :\` followed by Markdown list

**Grammar**:
```ebnf
NPListSent := NounPhrase VPHead ListKeyword ':' NEWLINE NPListItems EllipsisItem?
NPListItems := NPListItem+
NPListItem := '-' ListItem '.'
EllipsisItem := '-' ('...' | '…') '.'?
```

**List Keywords**: \`one of\`, \`any of\`, \`all of\`, \`none of\`

**Example**:
```sloj
The value is one of:
- 3.
- "pippo".
- xld.
```

**Open lists** (with ellipsis):
```sloj
The state is one of:
- idle.
- running.
- stopped.
- ...
```

**Rules**:
- Each item MUST end with \`.\`
- Optional ellipsis (\`- ...\` or \`- …\`) as final item indicates open list
- NP lists MUST be at end of sentence
- Items can be: full NPs, numbers, string literals, or proper nouns

### 4.3 Conditional Lists

**Syntax**: Conditional keyword + condition + \`:\` followed by Markdown list

**Grammar**:
```ebnf
ConditionalSentWithList := CondKeyword ConditionClause 'either'? ':' NEWLINE ConsequenceList
ConsequenceList := ConsequenceListItem+
ConsequenceListItem := '-' 'or'? 'then'? NounPhrase ConsequenceBody '.'
```

**Example**:
```sloj
If the user is authenticated:
  - The system grants access.
  - The session is created.
  - The event is logged.
```

**Rules**:
- Same list syntax as VP lists
- Each item is a complete consequence statement
- Supports parallel, sequential, exclusive, and inclusive coordination

### 4.4 While Lists

**Syntax**: \`while\` + condition + \`:\` followed by Markdown list

**Grammar**:
```ebnf
WhileSentWithList := 'while' ConditionClause ', either'? ':' NEWLINE ConsequenceList
```

**Example**:
```sloj
While the system is active:
  - The monitor checks status.
  - The logger records events.
```

**Rules**:
- Same list syntax as conditional lists
- Comma before \`:\` is optional (depends on \`either\` presence)

---

## 5. Fenced Code Blocks

Code blocks are **documentary elements** that appear at sentence level.

### 5.1 Syntax

**Triple backticks** or **triple tildes**

### 5.2 Grammar

```ebnf
CodeBlock := TripleBacktickBlock | TripleTildeBlock
TripleBacktickBlock := '```' SP* Language? NEWLINE CodeContent NEWLINE '```'
TripleTildeBlock := '~~~' SP* Language? NEWLINE CodeContent NEWLINE '~~~'
Language := <language_identifier>
CodeContent := <arbitrary_text>
```

### 5.3 Rules

1. Code blocks appear **between sentences**, not inline
2. Optional language identifier after opening fence (e.g., \`json\`, \`python\`, \`sql\`)
3. Content taken verbatim (no parsing)
4. Invisible to pronoun resolution
5. Do not participate in VP/NP coordination, relative clauses, or conditionals
6. Purely documentary (not part of formal semantics)

---

## 6. Annotations

Annotations provide metadata and appear on their own line(s) before sentences or code blocks.

### 6.1 Syntax

```
[[ content ]]
```

### 6.2 Grammar

```ebnf
Annotation := '[[' AnnotationContent ']]' NEWLINE
AnnotationContent := KeyValuePairs | FreeText
KeyValuePairs := KeyValuePair (',' SP* KeyValuePair)*
KeyValuePair := Identifier ':' SP* AnnotationValue
```

### 6.3 Forms

**Key-value pairs**:
```sloj
[[ id: REQ-001, priority: high ]]
The system must respond within 100 milliseconds.
```

**Free text**:
```sloj
[[ This requirement comes from the customer SLA. ]]
The system must be available 99.9% of the time.
```

**Multiple annotations**:
```sloj
[[ id: REQ-001 ]]
[[ priority: high ]]
[[ author: alice, date: 2024-01-15 ]]
The system must validate all inputs.
```

### 6.4 Rules

1. Appear on own line(s) immediately before the element they annotate
2. Non-normative (metadata only)
3. Do not affect formal semantics
4. Cannot be nested
5. Leading/trailing whitespace in values is trimmed

---

## 7. Forbidden Markdown in Sloj Text

The following Markdown constructs MUST NOT appear within Sloj sentences:

### 7.1 Emphasis and Formatting

\`*italic*\`, \`**bold**\`, \`~~strikethrough~~\`

**Rule**: Forbidden in Sloj text. Use plain text only.

**Rationale**: Formatting introduces ambiguity and has no semantic meaning in formal requirements.

### 7.2 Links

\`[link text](url)\`

**Rule**: Forbidden in Sloj text. Use string literals for URLs.

**Example**:
```sloj
The system must connect to "https://api.example.com".
```

### 7.3 Inline Code

**Rule**: Forbidden in Sloj text. Use plain text or string literals.

**Correct**:
```sloj
The system uses HTTP protocol.
The system uses the "HTTP" protocol.
```

### 7.4 Images

\`![alt text](image.png)\`

**Rule**: Forbidden in Sloj text. Images are documentary only.

---

## 8. Tables

### 8.1 Table Content is Free Text

**Rule**: All table content (headers and cells) is free text and never parsed as Sloj.

**Rationale**:
- Tables are typically for documentation/metadata, not requirements
- Requirements should be in prose paragraphs
- Keeps parsing simple and unambiguous
- Avoids ambiguity with periods in cells (abbreviations, decimals, etc.)

### 8.2 Example

```markdown
| ID | Description | Priority |
|----|-------------|----------|
| REQ-001 | Login feature | High |
| REQ-002 | Export data | Medium |

**REQ-001**: The system must authenticate users.

**REQ-002**: The user can export data.
```

- Table: All content is free text (metadata)
- Sentences after table: Sloj text (requirements)

### 8.3 Best Practice

Use tables for:
- Requirements metadata (ID, priority, status, owner)
- Cross-references
- Traceability matrices
- Summary information

Write actual requirements as Sloj sentences in paragraphs.

---

## 9. Parsing Model

### 9.1 Two-Phase Parsing

**Phase 1: Markdown Extraction**
1. Identify document structure (headings, paragraphs, lists, tables, code blocks)
2. Extract Sloj content:
   - Paragraph text (potential Sloj sentences)
   - Markdown lists introduced by \`:\` (VP/NP/conditional/while lists)
   - Annotations (\`[[ ]]\`)
   - Code blocks (as opaque documentary content)
3. Mark free text (headings, tables, blockquotes, etc.)
4. Preserve list structure and code block boundaries

**Phase 2: Sloj Parsing**
1. Parse extracted Sloj sentences
2. Integrate Markdown lists as syntactic elements
3. Associate annotations with following elements
4. Validate grammar and semantics
5. Treat code blocks as documentary (no parsing)

### 9.2 Sentence Detection

Within paragraph text, detect Sloj sentences:

1. Sentence ends with \`.\` (period)
2. Followed by whitespace or end of paragraph
3. Contains valid Sloj structure (NP + VP)

**Edge cases**:
- Abbreviations: "The system uses Dr. Smith's protocol." (valid sentence)
- Decimals: "The value is 3.14." (valid sentence)
- Multiple sentences: "The system validates. It logs errors." (two sentences)

---

## 10. Ambiguity Resolution

### 10.1 Colon (\`:\`) Disambiguation

The \`:\` character has multiple uses:

1. **Sloj list introducer** (end of line, followed by Markdown list)
2. **Annotation key-value separator** (within \`[[ ]]\`)
3. **List keyword component** (\`one of:\`, \`any of:\`, etc.)

**Resolution**:
- Context-dependent parsing
- End-of-line \`:\` followed by Markdown list → Sloj list syntax
- \`:\` within \`[[ ]]\` → annotation syntax
- \`:\` after list keyword → inline or Markdown list

### 10.2 Hyphen (\`-\`) Disambiguation

The \`-\` character has multiple uses:

1. **Markdown list item marker** (at start of line)
2. **Hyphen in compound words** (\`self-authenticate\`, \`multi-factor\`)
3. **Minus sign** (in numbers: \`-5\`)

**Resolution**:
- At start of line (after whitespace) → Markdown list item
- Within word → hyphen
- Before digit → minus sign

### 10.3 Period (\`.\`) Disambiguation

The \`.\` character has multiple uses:

1. **Sentence terminator**
2. **List item terminator**
3. **Decimal point** (in numbers: \`3.14\`)
4. **Ellipsis component** (\`...\`)

**Resolution**:
- After complete sentence → sentence terminator
- At end of list item → list item terminator
- Between digits → decimal point
- Three consecutive → ellipsis

### 10.4 Backtick Disambiguation

Backticks have multiple uses:

1. **Markdown inline code**
2. **Sloj string literal**

**Resolution**:
- In paragraph text: Forbidden (linter warning)
- In Sloj sentence: String literal
- In free text (headings, tables): Markdown inline code

---

## 11. Indentation and Whitespace

### 11.1 List Indentation

**Rule**: List items should be indented 2-4 spaces or 1 tab.

**Recommendation**: Use consistent indentation within a document.

### 11.2 Blank Lines

**In lists**: Blank lines between list items are allowed and improve readability.

**Between sentences**: Blank lines have no semantic meaning. Sentences are delimited by period.

### 11.3 Trailing Whitespace

**Rule**: Trailing whitespace on lines is ignored.

**Exception**: \`:\` must be the last non-whitespace character when introducing a list.

---

## 12. Validation and Linting

### 12.1 Markdown Structure Validation

Linters should validate:

1. **List structure**:
   - \`:\` at end of line is followed by Markdown list
   - List items have correct markers (\`-\`, \`or\`, \`then\`, \`nor\`)
   - List items end with \`.\`
   - VP/NP lists are final elements

2. **Code block structure**:
   - Properly fenced (matching backticks or tildes)
   - Appear at sentence level (not mid-sentence)
   - Optional language identifier is valid

3. **Annotation structure**:
   - Properly delimited (\`[[ ]]\`)
   - On own line(s)
   - Immediately before annotated element
   - Valid key-value syntax or free text

### 12.2 Markdown-Sloj Boundary Validation

Linters should warn about:

1. **Markdown formatting in Sloj sentences**:
   - \`*italic*\`, \`**bold**\` within sentences
   - \`[links](url)\` within sentences
   - Inline code within sentences

2. **Incomplete integration**:
   - \`:\` at end of line not followed by list
   - Markdown list not preceded by \`:\` introducer
   - List items without proper terminators

3. **Ambiguous constructs**:
   - Unclear whether content is Sloj or free text
   - Mixed formatting styles

### 12.3 Check Categories

- \`markdown-structure\`: List syntax, code blocks, annotations
- \`markdown-boundary\`: Formatting in Sloj sentences
- \`markdown-consistency\`: Indentation, blank lines

---

## 13. Tool Implementation Guidelines

### 13.1 Parser Requirements

1. **Markdown-aware**: Recognize Markdown structure
2. **Two-phase**: Extract Sloj content, then parse
3. **Preserve structure**: Maintain list hierarchy and code block boundaries
4. **Associate metadata**: Link annotations to following elements

### 13.2 Editor Integration

1. **Syntax highlighting**: Distinguish Sloj text from free text
2. **Completion**: Suggest Sloj keywords in appropriate contexts
3. **Validation**: Real-time checking of Sloj syntax and Markdown integration
4. **Formatting**: Auto-indent lists, normalize whitespace

### 13.3 Conversion Tools

1. **Extract**: Pure Sloj from Markdown (strip formatting)
2. **Embed**: Sloj into Markdown (add structure)
3. **Validate**: Round-trip conversion
4. **Transform**: Sloj-in-Markdown to other formats (HTML, PDF, LaTeX)

---

## 14. References

- [Sloj Specification](sloj-spec-doc.md) - Core language definition
- [Sloj Grammar](sloj-spec-grammar.bnf) - Formal EBNF grammar
- [Sloj Examples](sloj-examples.md) - Comprehensive examples

---

## Appendix A: Quick Reference

### Sloj Text (Parsed)
- Paragraph text (complete sentences)
- List items (when introduced by \`:\`)
- Annotation content

### Free Text (Not Parsed)
- Headings
- Tables (all content)
- Blockquotes
- Links, images
- Inline code (in free text)

### Integration Points
- VP lists: \`NP:\` + Markdown list
- NP lists: \`NP VP ListKeyword:\` + Markdown list
- Conditional lists: \`if/whenever/once/each time Condition:\` + Markdown list
- While lists: \`while Condition:\` + Markdown list
- Code blocks: triple backticks or tildes
- Annotations: \`[[ ]]\`

### Forbidden in Sloj Text
- Emphasis: \`*italic*\`, \`**bold**\`
- Links: \`[text](url)\`
- Inline code
- Images: \`![alt](url)\`

---

**Version**: 0.1  
**Status**: Draft  
**Last Updated**: 2026-02-02
