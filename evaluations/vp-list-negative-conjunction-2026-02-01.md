# VP List Negative Conjunction Addition

**Date**: 2026-02-01  
**Feature**: Negative conjunction for VP lists (3+ items)  
**Status**: ✅ Complete

---

## Summary

Added support for negative conjunction in VP lists to express that none of the listed VPs are true. This completes the symmetry with the existing inline `neither VP₁, nor VP₂` form.

---

## Changes Made

### 1. Spec Document (`spec/sloj-spec-doc.md`)

**Section 3.7 - VP List Coordination table updated:**

Added row:
```
| Negative conjunction | `NP neither:` + `- nor VP` items | None of the VPs |
```

Updated rules to include `nor` marker:
```
**Rules:** First item is unmarked; subsequent items carry markers (`then`/`or`/`nor`).
```

### 2. Grammar File (`spec/sloj-spec-grammar.bnf`)

**VPList rule updated:**
```ebnf
VPList := 'either'? ':' NEWLINE VPListItems
        | 'neither' ':' NEWLINE VPListItems
```

**VPListItem rule updated:**
```ebnf
VPListItem := '-' 'or'? 'then'? 'nor'? VerbPhrase '.'
```

### 3. Examples File (`spec/sloj-examples.md`)

**Quick Reference Index updated:**
- Added entry: `VP list negative | [7.5] | 3.7 | VPList | neither:, nor, none`
- Renumbered subsequent sections (7.5→7.6, 7.6→7.7)

**Table of Contents updated:**
- Added: `7.5 [Negative Conjunction](#75-negative-conjunction)`
- Renumbered subsequent sections

**New example section added (7.5):**
```sloj
The cat neither:
  - purrs at strangers.
  - nor jumps on the bed.
  - nor eats fish.
```
*None of the VPs hold*

---

## Syntax

### Form
```
NP neither:
  - VP₁.
  - nor VP₂.
  - nor VP₃.
  ...
```

### Semantics
**Logic**: ¬VP₁ ∧ ¬VP₂ ∧ ¬VP₃ ∧ ...

All VPs are negated and conjoined (none of them are true).

### Rules
1. Subject followed by `neither:`
2. First list item is unmarked: `- VP.`
3. Subsequent items use `nor` marker: `- nor VP.`
4. Each item ends with `.`
5. VP list must be final element of sentence

---

## Complete VP List Coordination System

| Form | Syntax | Logic | Meaning |
|------|--------|-------|---------|
| Parallel conjunction | `NP:` + unmarked | ∧ | All VPs true, unordered |
| Sequential conjunction | `NP:` + `- then VP` | ∧ + order | All VPs true, ordered |
| Exclusive disjunction | `NP either:` + `- or VP` | ⊕ | Exactly one VP true |
| Inclusive disjunction | `NP:` + `- or VP` | ∨ | At least one VP true |
| **Negative conjunction** | **`NP neither:` + `- nor VP`** | **¬∧** | **None of the VPs true** |

---

## Consistency with Existing Forms

### Inline VP Coordination (2 items)
```sloj
"The system neither validates the input, nor logs the error."
```
Logic: ¬validates ∧ ¬logs

### VP List Coordination (3+ items) - NEW
```sloj
The system neither:
  - validates the input.
  - nor logs the error.
  - nor sends a notification.
```
Logic: ¬validates ∧ ¬logs ∧ ¬sends

### Sentence-Level Coordination (2 items)
```sloj
"Neither a user logs-in, nor the connection times-out."
```
Logic: ¬(user logs-in) ∧ ¬(connection times-out)

---

## Examples

### Basic Example
```sloj
The cat neither:
  - purrs at strangers.
  - nor jumps on the bed.
  - nor eats fish.
```
*The cat doesn't purr, doesn't jump, and doesn't eat fish*

### With Coordination
```sloj
The server neither:
  - accepts new connections.
  - nor processes pending requests.
  - nor sends status updates.
```
*The server does none of these actions*

### After Sentence Coordination
```sloj
The alarm triggers, and the system neither:
  - logs the event.
  - nor notifies the admin.
  - nor shuts down.
```
*Alarm triggers, but system does none of the listed actions*

---

## Grammar Impact

**Minimal change** as predicted:
1. Added `'neither'` alternative to `VPList` rule
2. Added `'nor'?` option to `VPListItem` rule

The grammar remains liberal; semantic layer disambiguates coordination type based on presence of `either`/`neither` and markers (`or`/`then`/`nor`).

---

## Rationale

### Completeness
This addition completes the VP list coordination system by providing a list form for negative conjunction, matching the existing inline `neither...nor` form.

### Symmetry
All VP coordination forms now have both inline (2 items) and list (3+ items) variants:

| Type | Inline (2 items) | List (3+ items) |
|------|------------------|-----------------|
| Parallel conjunction | `VP && VP` | `NP:` + unmarked |
| Sequential conjunction | `VP && then VP` | `NP:` + `- then VP` |
| Exclusive disjunction | `either VP, or VP` | `NP either:` + `- or VP` |
| Inclusive disjunction | `VP &&/ VP` | `NP:` + `- or VP` |
| **Negative conjunction** | **`neither VP, nor VP`** | **`NP neither:` + `- nor VP`** |

### User Request
Feature was identified as missing by user during review of VP list coordination forms.

---

## Testing Considerations

When implementing:
1. Verify first item is unmarked
2. Verify subsequent items have `nor` marker
3. Verify all VPs are negated in semantic interpretation
4. Verify conjunction of negations (not disjunction)
5. Test with 2, 3, 4+ items
6. Test after sentence coordination
7. Test after inline VP coordination (e.g., `VP &&:`)

---

## ion

The negative conjunction VP list form has been successfully added to Sloj, completing the VP list coordination system and maintaining consistency with existing inline forms.

**Files modified:**
- `spec/sloj-spec-doc.md`
- `spec/sloj-spec-grammar.bnf`
- `spec/sloj-examples.md`

**Grammar impact:** Minimal (2 small additions)  
**Consistency:** Excellent (matches existing patterns)  
**Completeness:** System now complete
