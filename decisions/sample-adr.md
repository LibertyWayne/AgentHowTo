# ADR-002: Dominant Contract Selection Algorithm

> **Status:** Accepted · **Date:** 2026-04-26 · **Revisit:** 2026-07-26
>
> *Architecture Decision Record — a real example of how the agent documents technical choices.*

---

## Context · 背景

Chinese futures markets have multiple concurrent contracts for the same variety (e.g., CU2505, CU2506, CU2507...). For continuous price series, we need to select a "dominant" (主力) contract for each variety on each day.

**Previous approach (v5):** Select the contract with the highest volume, plus a month-based preference for nearby months.

**Problem:** Using calendar month proximity (`retired_ym`) to prefer near-month contracts caused incorrect selections. A far-month contract with higher volume was sometimes rejected because a near-month contract had "close enough" volume — but the near-month contract was about to expire and had decaying liquidity.

---

## Decision · 决策

**Adopt v6 algorithm: Pure volume maximum + 115% stickiness.**

```
For each variety on each trading day:

1. Find the contract with maximum volume → this is the candidate
2. Check stickiness: if yesterday's dominant contract
   has volume ≥ candidate_volume × 1.15, keep yesterday's
3. Otherwise, switch to the candidate

REMOVED: retired_ym (month proximity preference)
```

| Component | Logic |
|-----------|-------|
| Primary selector | Maximum daily volume |
| Stickiness check | Keep previous dominant if it has ≥115% of candidate volume |
| Retired feature | `retired_ym` — month proximity was misleading (month number ≠ time to expiration) |

---

## Rationale · 理由

| Option | Pros | Cons | Verdict |
|--------|------|------|---------|
| Pure max volume | Simple, objective | Choppy switching near roll | Too unstable |
| Max volume + month proximity (v5) | Smoother transitions | Wrong when far-month dominates | **Rejected** |
| Max volume + 115% stickiness (v6) | Smooth + correct | Slightly more complex | **Accepted** |

**Why 115%?** Empirically calibrated. Lower thresholds (105%) caused excessive switching; higher thresholds (130%) caused delayed rolls. 115% produced **1.29% inconsistency rate** against manually verified roll dates — the best of all tested values.

---

## Consequences · 后果

### Positive
- Eliminated the `retired_ym` bug class entirely
- Stateless — no need to track contract expiration schedules
- 1.29% inconsistency rate is acceptable for production use

### Negative
- Stickiness means the algorithm occasionally stays on a decaying contract for 1 extra day
- Pure volume selection doesn't consider open interest, which some researchers prefer

### Neutral
- Continuous series views (v_dominant_continuous, v_dominant_adj) need to be regenerated after algorithm changes
- Documentation updated in `futures/docs/futures_new_dev_notes.md`

---

## Revisit Plan · 回顾计划

**Date:** 2026-07-26 (3 months)

**Questions to answer:**
1. Is the 1.29% inconsistency rate stable or drifting?
2. Should open interest be a tiebreaker?
3. Are there varieties where 115% stickiness is too high or too low?

---

> **Template:** [ADR Template](../templates/adr-template.md) — use this for your own decisions
