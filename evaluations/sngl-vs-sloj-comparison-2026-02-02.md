# Evaluation: SNGL vs Sloj Feature Comparison

**Date:** 2026-02-02  
**Purpose:** Identify SNGL features not present in Sloj and assess their value

---

## Executive Summary

Sloj is a direct successor to SNGL, retaining most of its core features while making some deliberate simplifications. After a detailed comparison, I identified **8 features** present in SNGL that are absent or different in Sloj. Of these, **3 are potentially useful additions** worth considering, while **5 appear to be intentional simplifications** that align with Sloj's design goals.

---

## Feature Comparison Matrix

| Feature | SNGL | Sloj | Assessment |
|---------|------|------|------------|
| Symbolic NP coordination (`&`, `/`, `&/`) | Yes | Yes | Identical |
| Symbolic VP coordination (`&&`, `&& then`, `&&/`) | Yes | Yes | Identical |
| Exclusive VP (`either...or`) | Yes | Yes | Identical |
| Negative VP (`neither...nor`) | Yes | Yes | Identical |
| Guillemets for scope | Yes | Yes | Identical |
| Present perfect | Yes | Yes | Identical |
| Conditionals | Yes | Yes | Identical |
| Temporal/future statements | Yes | Yes | Identical |
| Deontic modals | Yes | Yes | Identical |
| Comparisons | Yes | Yes | Identical |
| Measurement phrases | Yes | Yes | Identical |
| Mass comparisons | Yes | Yes | Identical |
| Partitive constructions | Yes | Yes | Identical |
| String literals | Yes | Yes | Identical |
| Purpose clauses | Yes | Yes | Identical |
| Annotations | Yes | Yes | Sloj uses `[[ ]]`; SNGL uses `[[ ]]` or `〚〛` |
| Lists/enumeration | Yes | Yes | Sloj: `one of:`, `any of:`, `all of:`, `none of:` vs SNGL: `:(...)` |
| VP list coordination | Yes | Yes | Identical |
| Named references/binding | Yes | Yes | Sloj omits `aka` form |
| **Gender-neutral pronouns** | `yey/yem/yeir/yeirs/yemself` | `he/she/it/they` | **Missing in Sloj** |
| **Sentence negation** | `it is false that S` | Not supported | **Deferred in Sloj** |
| **`except` determiner** | Yes | Partially | Sloj has `except` suffix; SNGL has `except` determiner |
| **VP coordination precedence** | `&&` > `&& then` > `&&/` | Equal precedence | **Design difference** |
| **Dual-delimiter strings** | `"..."`, `` `...` ``, `⸄...⸅` | `"..."`, `` `...` `` | Minor omission |
| **Open/Closed lists** | Explicit semantics | No distinction | **Simplification** |
| **There-sentence coordination** | Same as simple | Limited | Minor difference |
| **Unique noun uniqueness scope** | `of`-clause scope | Similar | Identical intent |

---

## Detailed Analysis of Missing/Different Features

### 1. Gender-Neutral Pronouns (SNGL: `yey/yem/yeir/yeirs/yemself`)

**SNGL Approach:**
```
- yey (subjective): "A customer logs in. Yey sees the dashboard."
- yem (objective): "The system notifies yem."
- yeir (possessive determiner): "Yey updates yeir profile."
- yeirs (possessive pronoun): "The account is yeirs."
- yemself (reflexive): "A user authenticates yemself."
```

**Sloj Approach:**
- Sloj uses standard English pronouns: `he/she/it/they`
- Gender is determined by recognized names (Male/Female) or Neutral for others
- Uses `self-` verb prefix for reflexive: "Bob self-authenticates."

**Assessment:** 
- **Value:** LOW to MODERATE
- **Rationale:** SNGL's Elverson pronouns provide unambiguous singular reference when gender is unknown/irrelevant, avoiding the grammatical ambiguity of singular "they" (which SNGL explicitly forbids). However, they introduce unfamiliar syntax that may reduce readability. Sloj's approach of using `self-` prefixes for reflexive actions and relying on explicit binding for disambiguation is simpler.
- **Recommendation:** Could be reconsidered if users encounter significant pronoun resolution issues with gender-neutral entities. The `self-` verb prefix is a reasonable alternative for reflexive cases.

---

### 2. Sentence Negation (`it is false that S`)

**SNGL:**
```
It is false that a customer inserts a card.
```
Negates the entire proposition, including quantifiers and coordination.

**Sloj:**
- VP negation only: `does not`, `do not`, `is not`, `are not`
- NP negation: `no NP`

**Assessment:**
- **Value:** LOW (for current use cases)
- **Status:** Already deferred in Sloj (task 2.9, marked `[!]`)
- **Rationale:** VP negation covers most practical requirements scenarios. Sentence negation would be needed mainly for complex propositions with coordination or quantifiers where VP negation is insufficient.
- **Recommendation:** Keep deferred. If future users need to negate complex coordinated statements, revisit.

---

### 3. `except` Determiner

**SNGL:**
```
Every user except administrators must authenticate.
```
The `except` is a negative existential determiner modifying the NP.

**Sloj:**
```
Every user except administrators must authenticate.
```
Sloj has `except` as a suffix: `NP except NP` and `NP except the one/ones that VP`

**Assessment:**
- **Value:** EQUIVALENT
- **Rationale:** Both languages support the same semantic construct, just with slightly different grammatical categorization. Sloj's suffix approach (`ExceptSuffix` in grammar) achieves the same result.
- **Recommendation:** No change needed.

---

### 4. VP Coordination Precedence Rules

**SNGL:**
```
Precedence: && > && then > &&/
- "X P && Q && then S" → "X <<P && Q>> && then S"
- "X P &&/ Q && then S" → "X P &&/ <<Q && then S>>"
```

**Sloj:**
```
All VP operators have equal precedence, left-associative.
- "A && B &&/ C" → "((A && B) &&/ C)"
```

**Assessment:**
- **Value:** DESIGN TRADEOFF
- **Rationale:** SNGL's precedence rules reduce guillemet usage for common patterns but add complexity. Sloj's uniform left-associativity is simpler and more predictable, though it may require more guillemets.
- **Recommendation:** Keep Sloj's simpler approach. The predictability of "all equal, left-associative" outweighs the convenience of implicit precedence.

---

### 5. Triple-Delimiter Strings (`⸄...⸅`)

**SNGL:**
```
Strings are enclosed in: "...", `...`, or ⸄...⸅
```

**Sloj:**
```
Strings use: "..." or `...`
```

**Assessment:**
- **Value:** VERY LOW
- **Rationale:** Two delimiter options are sufficient for escaping needs. The `⸄⸅` delimiters are obscure Unicode characters that most users cannot easily type.
- **Recommendation:** No change needed.

---

### 6. Open-World vs Closed-World Lists

**SNGL:**
```
Closed-World (exhaustive): "There can only be three states: (idle, running, stopped.)"
Open-World (extensible): "The vulnerabilities include: (SQL-injection, XSS.)"
```
Uses surrounding text to distinguish semantics.

**Sloj (RESOLVED):**
```
Exhaustive (default): The state is one of: idle, running, stopped.
Open (with ellipsis): The state is one of: idle, running, stopped, ...
```
Uses trailing ellipsis (`...` or `…`) to indicate open lists.

**Assessment:**
- **Value:** RESOLVED
- **Rationale:** Sloj now supports both exhaustive (closed-world) and open (extensible) lists through a simple, intuitive syntax. Lists without ellipsis are exhaustive by default (the safer assumption for requirements). Adding `...` or `…` as the final element indicates the list is extensible.
- **Status:** Implemented in spec Section 8.11.4, grammar (`Ellipsis`, `EllipsisItem`), and examples Section 31.15.

---

### 7. `aka` Binding Form

**SNGL:**
```
A user, aka Alice, calls another user, aka Bob.
```

**Sloj:**
```
A user, named Alice, ...
A user, called Alice, ...
A user Alice ...  (implicit)
A user, Alice, ... (appositive)
```

**Assessment:**
- **Value:** VERY LOW
- **Rationale:** `aka` is redundant with `named` and `called`. The grammar note in Sloj explicitly states: "The 'aka' binding form from Sngl is not supported in Sloj."
- **Recommendation:** No change needed. Intentional simplification.

---

### 8. Interpretation Rules Documentation

**SNGL Section 8** provides extensive interpretation rules:
- Reference resolution tie-breaking
- Attachment rules
- Scope and precedence defaults
- Agreement rules
- Temporal interpretation

**Sloj:**
- Rules are distributed throughout spec sections
- Less explicit formalization of disambiguation

**Assessment:**
- **Value:** HIGH (for tooling)
- **Rationale:** SNGL's consolidated interpretation rules section would be valuable for implementing parsers and semantic analyzers.
- **Recommendation:** Consider adding an "Interpretation Rules" appendix to Sloj spec, consolidating the determinism rules. This is a documentation improvement, not a language change.

---

## Summary of Recommendations

### Worth Adding to Sloj

| Feature | Priority | Effort | Rationale |
|---------|----------|--------|-----------|
| Interpretation Rules appendix | Medium | Low | Documentation improvement for tooling |
| ~~`only` modifier for lists~~ | ~~Low~~ | ~~Low~~ | ~~Closed-world semantics for enumerations~~ |

**Note:** Open/closed list distinction was resolved using ellipsis syntax (`...` or `…`) instead of an `only` modifier. See Section 6 above.

### Intentional Omissions (Keep As-Is)

| Feature | Rationale |
|---------|-----------|
| Elverson pronouns (`yey/yem`) | Adds unfamiliarity; `self-` prefix handles reflexive |
| Sentence negation | VP negation sufficient; deferred |
| VP precedence hierarchy | Equal precedence is simpler |
| Triple delimiters (`⸄⸅`) | Two delimiters sufficient |
| `aka` binding | Redundant with `named`/`called` |

---

## Conclusion

Sloj has successfully incorporated nearly all of SNGL's essential features while making some simplifications that improve learnability. The main gap is in **documentation structure**—specifically, the lack of a consolidated interpretation rules section that would help tool implementers.

The language itself is feature-complete for requirements specification. The open/closed list distinction has been resolved using ellipsis syntax. The remaining potential addition (interpretation rules appendix) is a documentation refinement rather than a missing capability.

**Overall Assessment:** Sloj is a well-designed successor to SNGL that retains the core value proposition (deterministic parsing, formal grounding) while being more approachable.

---

## Appendix: Changes Made

**2026-02-02:** Added ellipsis syntax for open-world lists:
- Spec: Section 8.11.4 (Open vs Exhaustive Lists)
- Grammar: `Ellipsis` and `EllipsisItem` productions
- Examples: Section 31.15 (Open Lists)
