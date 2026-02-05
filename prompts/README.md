# Sloj LLM Prompts

This directory contains prompts for enabling Large Language Models to work with Sloj (controlled natural language for requirements).

---

## Available Prompts

### 1. Reader Prompt (`sloj-reader-prompt.md`)

**Purpose**: Enable LLMs to correctly interpret and analyze Sloj requirements documents.

**Use cases**:
- Summarizing Sloj documents in plain English
- Answering questions about requirements
- Logical interpretation of formal constructs
- Generating test cases from requirements
- Compliance checking

**Token count**: ~2400 tokens

**Coverage**: 12 critical syntax warnings + interpretation guidelines

**Expected accuracy**: 85-90% for common reading tasks

---

### 2. Writer Prompt (`sloj-writer-prompt.md`)

**Purpose**: Enable LLMs to generate valid Sloj requirements from natural language input.

**Use cases**:
- Converting English requirements to Sloj
- Formalizing informal specifications
- Generating requirements from descriptions
- Translating between formats

**Token count**: ~3500 tokens

**Coverage**: 14 critical generation rules + validation checklist

**Expected accuracy**: 80-85% (with 1-3 validation iterations)

**Note**: Requires validation pipeline (parser/linter) to ensure correctness.

---

## Usage

### For Reader Tasks

```
[System Prompt]
<contents of sloj-reader-prompt.md>

[User Query]
Read this Sloj document and answer: <question>

<sloj document>
```

### For Writer Tasks

```
[System Prompt]
<contents of sloj-writer-prompt.md>

[User Query]
Convert this requirement to Sloj: <natural language requirement>
```

---

## Test Cases

See `test-cases-reader.md` and `test-cases-writer.md` for validation tests.

---

## Prompt Versions

**Current version**: 1.0 (2026-02-05)

**Changelog**:
- v1.0: Initial release with reader and writer prompts

---

## Evaluation

See `../evaluations/` for detailed analysis:
- `sloj-reader-llm-prompt-approach.md` - Reader prompt evaluation
- `sloj-writer-llm-prompt-approach.md` - Writer prompt evaluation

---

## Future Enhancements

Planned improvements:
1. **Tiered prompts** (minimal, standard, comprehensive)
2. **Shared reference layer** (RAG-based knowledge base)
3. **Domain-specific variants** (with project lexicons)
4. **Model-specific tuning** (GPT-4, Claude, etc.)
5. **Few-shot examples library** (curated conversion examples)

---

## Contributing

When updating prompts:
1. Test changes with test cases
2. Measure accuracy impact
3. Update version number and changelog
4. Document any new rules or warnings
5. Update expected token counts

---

## License

TBD
