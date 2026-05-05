# ADR Template

> Architecture Decision Record — for documenting technical choices with context, options, and revisit plans.

```markdown
# ADR-NNN: [Short Title]

> **Status:** Proposed | Accepted | Deprecated | Superseded
> **Date:** YYYY-MM-DD
> **Revisit:** YYYY-MM-DD (optional — for decisions that should be re-evaluated)

---

## Context · 背景

What is the problem? Why does a decision need to be made now?
Include constraints, stakeholders, and any time pressure.

## Decision · 决策

What did we choose? Be specific — include parameters, thresholds, configurations.

## Options Considered · 备选方案

| Option | Pros | Cons | Verdict |
|--------|------|------|---------|
| Option A | ... | ... | Accepted / Rejected |
| Option B | ... | ... | Accepted / Rejected |
| Option C | ... | ... | Accepted / Rejected |

## Rationale · 理由

Why this option over the others? Include:
- Empirical data (if tested)
- Principles that guided the choice
- Trade-offs we explicitly accepted

## Consequences · 后果

### Positive
- ...
### Negative
- ...
### Neutral
- ...

## Revisit Plan · 回顾计划

**Date:** YYYY-MM-DD
**Questions to answer:**
1. Was the decision correct?
2. What would we do differently?
3. Are there new options available?
```

---

## When to Write an ADR

| Trigger | Example |
|---------|---------|
| Choosing between 2+ technical approaches | DuckDB vs PostgreSQL |
| Changing a core algorithm | Dominant contract v5 → v6 |
| Adopting a new tool or dependency | Adding AKShare to data pipeline |
| Retiring a feature | Removing `retired_ym` from contract selection |

## When NOT to Write an ADR

- Trivial choices ("which Python version?")
- Reversible decisions with no learning value
- Things already documented in code comments

---

> **See also:** [Sample ADR](../decisions/sample-adr.md) — real example
