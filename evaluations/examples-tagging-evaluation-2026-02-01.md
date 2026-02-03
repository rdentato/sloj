# Examples File Tagging Evaluation

**Date**: 2026-02-01  
**File**: `spec/sloj-examples.md`  
**Purpose**: Evaluate if examples have proper references/tagging for easy lookup

---

## Current State Analysis

### ✅ Strengths:
1. **Clear section numbering** (1-25) matching spec structure
2. **Hierarchical organization** with subsections (e.g., 1.1, 1.2)
3. **Consistent formatting** with code blocks and explanations
4. **Logical grouping** by grammatical feature

### ⚠️ Weaknesses:

#### 1. **No Index/Table of Contents**
- Users must scroll through entire file to find specific features
- No quick reference guide at the top

#### 2. **No Cross-References to Spec**
- Examples don't explicitly reference spec sections
- Example: Section 3 in examples should reference "Spec Section 3"

#### 3. **No Searchable Tags**
- Original file had tags like `# [temporal-basic-will]` which were removed
- These tags enabled quick text search
- Current format requires knowing section numbers

#### 4. **No Grammar Rule References**
- Examples don't reference corresponding EBNF rules
- Would help implementers cross-reference

#### 5. **No Feature Keywords**
- Missing metadata like: `Keywords: &&, parallel, conjunction, VP`
- Would enable better searchability

---

## Recommended Improvements

### Option A: Add Inline Tags (Minimal Change)

Add searchable tags to each example while keeping current structure:

```markdown
### 3.1 VP Operators
<!-- Tags: VP-coordination, &&, parallel-conjunction -->
<!-- Spec: Section 3.1 | Grammar: VerbConj -->

```sloj
"The system validates the input && logs the result."
```
*Parallel conjunction (`&&`): simultaneous/unordered*
```

**Pros**: 
- Minimal disruption to current structure
- Enables text search
- HTML comments don't clutter reading

**Cons**: 
- Still requires manual search
- No centralized index

---

### Option B: Add Table of Contents with Tags (Recommended)

Add comprehensive TOC at the beginning with searchable tags:

```markdown
# Sloj Language Examples

**Version**: 0.1  
**Date**: 2026-02-01

---

## Quick Reference Index

| Feature | Section | Spec Ref | Grammar Ref | Keywords |
|---------|---------|----------|-------------|----------|
| Sentence coordination | 1.2 | Spec 2.1 | DisjunctSent, ConjunctSent | `, and`, `, or`, `, but`, DNF |
| VP parallel conjunction | 3.1 | Spec 3.1 | VerbConj | `&&`, parallel, simultaneous |
| VP sequential conjunction | 3.1 | Spec 3.1 | VerbConj | `&& then`, sequential, ordered |
| VP inclusive disjunction | 3.1 | Spec 3.1 | VerbConj | `&&/`, inclusive, or |
| VP exclusive disjunction | 3.2 | Spec 3.1 | EitherVP | `either`, `or`, exclusive |
| VP negative conjunction | 3.2 | Spec 3.1 | NeitherVP | `neither`, `nor` |
| Guillemets grouping | 3.3 | Spec 3.2, 9 | GroupBody | `«»`, precedence, override |
| Adverb placement | 4.1 | Spec 3.4 | SimpleVPCore | pre-verbal, post-verbal |
| Adverb scope | 4.3 | Spec 3.4 | GroupVPCore | scope, guillemets |
| Modal must | 5.1 | Spec 3.5 | Modal | `must`, obligation |
| Modal must not | 5.1 | Spec 3.5 | Modal | `must not`, prohibition |
| Modal should | 5.1 | Spec 3.5 | Modal | `should`, recommendation |
| Modal able to | 5.2 | Spec 3.5 | Modal | `is able to`, `are able to`, capability |
| VP negation | 6 | Spec 3.6 | VPNeg | `does not`, `do not` |
| VP list parallel | 7.1 | Spec 3.7 | VPList | `:`, markdown, parallel |
| VP list sequential | 7.2 | Spec 3.7 | VPList | `then`, sequential, ordered |
| VP list exclusive | 7.3 | Spec 3.7 | VPList | `either:`, `or`, exclusive |
| VP list inclusive | 7.4 | Spec 3.7 | VPList | `or`, inclusive |
| NP conjunction | 8.1 | Spec 7.1 | NounConj | `&`, both |
| NP exclusive disjunction | 8.1 | Spec 7.1 | NounConj | `/`, one-not-both |
| NP inclusive disjunction | 8.1 | Spec 7.1 | NounConj | `&/`, one-both-either |
| Proper noun | 9.1 | Spec 7.3 | ProperNP | capitalized, no-determiner |
| Unique noun | 9.2 | Spec 7.3 | UniqueNP | capitalized, with-determiner |
| Mass noun | 9.3 | Spec 7.3 | MassNP | uncountable, mass-determiner |
| Binding implicit | 10.1 | Spec 7.6 | Binding | juxtaposition |
| Binding explicit | 10.1 | Spec 7.6 | Binding | `, named`, `, called` |
| Pronoun resolution | 11.1 | Spec 7.5 | PronounNP | `he`, `she`, `it`, `they` |
| Reflexive action | 11.2 | Spec 7.5 | SelfVerb | `self-` prefix |
| Relative clause | 12.1 | Spec 3.8 | RelativeClause | `that` |
| Present perfect | 12.2 | Spec 3.8 | PerfectVP | `has`, `have`, prior-event |
| Present perfect negation | 12.3 | Spec 3.8 | PerfectNeg | `has not`, `has never` |
| Of-clause | 13.1 | Spec 3.3 | OfClause | `of`, binds-to-noun |
| Non-of PP | 13.2 | Spec 3.3 | NonOfPrepPhrase | binds-to-verb, guillemets |
| Partitive proportion | 14.1 | Spec 7.7 | ProportionPartitiveHead | `most of`, `%`, mass |
| Partitive count | 14.2 | Spec 7.7 | CountPartitiveHead | `N of the`, count |
| Exception suffix | 15 | Spec 7.8 | ExceptSuffix | `except` |
| Existential | 16 | Spec 5 | ExistentialSent | `there is`, `there are` |
| Conditional if | 17.1 | Spec 6 | ConditionalSent | `if`, `then` |
| Conditional exception | 17.2 | Spec 6.2 | ConditionalSent | `except when`, `except if` |
| Conditional list | 17.3 | Spec 6.3 | ConditionalSentWithList | `:`, markdown |
| Whenever | 18 | Spec 6 | CondKeyword | `whenever`, universal-reactive |
| Once | 19 | Spec 6 | CondKeyword | `once`, one-time-trigger |
| Each time | 20 | Spec 6 | CondKeyword | `each time`, repeated-trigger |
| Otherwise | 21 | Spec 6.1 | OtherwiseSent | `Otherwise,` |
| Passive voice | 22 | Spec 3.10 | VerbArg | `is/are` + PastPart |
| String literal | 23 | Spec 8 | StringLiteral | `"..."`, `` `...` ``, invisible |
| Code block | 24 | Spec 8.4 | CodeBlock | ` ``` `, `~~~`, invisible |
| Future will | 25.1 | Spec 4.1 | FutureAux | `will`, `will always`, `will never` |
| Temporal scope | 25.2 | Spec 4.2 | TemporalPrefix | `before`, `after` |
| Temporal continuous | 25.3 | Spec 4.3 | TemporalSuffix | `until`, `unless`, `while` |
| Temporal sequence | 25.4 | Spec 4.4 | TemporalSequence | `first`, `then`, `finally` |
| Temporal duration | 25.5 | Spec 4.5 | Duration | `within`, `for`, `every` |

---

## Table of Contents

1. [Basic Sentence Structure](#1-basic-sentence-structure)
   - 1.1 [Simple Sentences](#11-simple-sentences)
   - 1.2 [Sentence Coordination](#12-sentence-coordination)
2. [Unified Verb Syntax](#2-unified-verb-syntax)
   - 2.1 [Copula Forms](#21-copula-forms)
   - 2.2 [Verbs with Arguments](#22-verbs-with-arguments)
3. [Verb Phrase (VP) Coordination](#3-verb-phrase-vp-coordination)
   - 3.1 [VP Operators](#31-vp-operators)
   - 3.2 [Fixed English VP Forms](#32-fixed-english-vp-forms)
   - 3.3 [VP Precedence and Grouping](#33-vp-precedence-and-grouping)
...
```

**Pros**:
- Comprehensive searchable index
- Quick lookup by feature, keyword, or reference
- Links to sections for easy navigation
- Cross-references to spec and grammar

**Cons**:
- Larger file
- Requires maintenance when adding examples

---

### Option C: Separate Index File (Alternative)

Create `spec/sloj-examples-index.md` with the reference table, keep examples file clean.

**Pros**:
- Keeps examples file readable
- Centralized reference
- Easy to maintain separately

**Cons**:
- Two files to manage
- Users need to know about index file

---

## Recommendation

**Implement Option B** (Add TOC with tags to examples file)

### Rationale:
1. **Single source of truth** - Everything in one file
2. **Searchable** - Keywords enable Ctrl+F search
3. **Cross-referenced** - Links to spec and grammar
4. **Navigable** - TOC with anchor links
5. **Comprehensive** - Covers all use cases

### Implementation:
1. Add Quick Reference Index table at top
2. Add full Table of Contents with links
3. Keep current example structure unchanged
4. Optionally add HTML comment tags for additional searchability

---

## Search Use Cases

With the recommended approach, users can:

1. **Search by operator**: Ctrl+F "`&&`" → finds Quick Reference entry → jumps to section
2. **Search by concept**: Ctrl+F "parallel" → finds multiple relevant entries
3. **Search by spec section**: Ctrl+F "Spec 3.1" → finds all examples for that section
4. **Search by grammar rule**: Ctrl+F "VerbConj" → finds grammar-related examples
5. **Browse by category**: Use TOC to navigate to feature area

---

## Conclusion

Current examples file is well-organized but lacks discoverability features. Adding a Quick Reference Index and TOC would make it significantly more useful for:
- **Writers** looking for syntax examples
- **Implementers** cross-referencing grammar rules
- **Learners** exploring language features
- **Reviewers** validating spec coverage

**Estimated effort**: 1-2 hours to create comprehensive index and TOC.
