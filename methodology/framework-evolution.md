# Methodology Evolution · 方法论进化

> *How the agent's analytical framework improves through real mistakes — not through planned upgrades, but through error-driven iteration.*

---

## The Principle · 原则

> **Every version bump has a corpse.** The agent doesn't upgrade because it "read a better paper." It upgrades because it made a mistake, traced the root cause, and changed the framework so that mistake can't happen again.

---

## Version History

### v1.0 — Three-Layer Architecture (Initial)

```
Macro → Industry → Technical
  ↑        ↑          ↑
direction  space      timing
```

**Design:** Classical top-down. Macro sets the direction, industry determines relative value, technicals pick entry/exit.

**Problem discovered:** All commodities treated identically. A policy-driven variety (lithium carbonate) doesn't behave like a supply-driven one (copper).

---

### v2.0 — Commodity-Type Classification

```
                   ┌─ Supply-driven (CU, ZN, AL → cost curve matters)
Macro ──→ Type ───┼─ Demand-driven (RB, I → macro cycle matters)
                   └─ Policy-driven (LC, SI → regulatory signal matters)
                            │
                            ▼
                       Industry Analysis
                            │
                            ▼
                       Technicals
```

**Added:** Commodity classification layer between macro and industry. Each type gets a different analytical weight distribution.

**Trigger:** Lithium carbonate analysis on 2026-04-26 — misread a policy announcement as a supply signal. The framework didn't distinguish between supply-side and policy-side catalysts.

---

### v3.0 — Common-Factor Contamination Detection

```
Before analysis:
  ┌──────────────────────────────────────┐
  │ Check: Is this move variety-specific │
  │        or factor-driven?             │
  │                                      │
  │  • Macro factor (USD, rates)         │
  │  • Sector factor (nonferrous beta)   │
  │  • Idiosyncratic (variety-specific)  │
  └──────────────────────────────────────┘
```

**Added:** Pre-analysis common-factor screening. Before attributing a price move to variety fundamentals, check if it's explained by shared factors.

**Trigger:** Aluminum analysis error — attributed a rally to supply tightness when the entire nonferrous complex was rising on USD weakness. The variety-specific story was noise; the factor story was signal.

---

### v4.0 — Cost Three-Layer Model

```
Price
  │
  ├── C1: Cash Operating Cost     ← hard floor (mines shut below this)
  │
  ├── A1: Full Production Cost    ← soft floor (includes depreciation)
  │                                ← statistical center of gravity
  │
  └── A2: Marginal Cost           ← ceiling for surplus regime
                                  ← floor for deficit regime
```

**Added:** Standardized cost model with three layers (C1, A1, A2) for supply-driven commodities. Replaces ad-hoc "cost support" analysis.

**Trigger:** Aluminum oxide analysis — couldn't explain why AO traded below "cost" without a supply response. The old framework only had one cost layer; reality has three.

---

### v4.1 — Policy-Driven Variety Special Rules (Current)

```
Policy-driven varieties (LC, SI, PS):
  ┌─────────────────────────────────────┐
  │ 1. Regulation is the PRIMARY driver │
  │ 2. Cost models are SECONDARY        │
  │ 3. Technicals are TERTIARY          │
  │ 4. Policy calendar > earnings calendar│
  └─────────────────────────────────────┘
```

**Added:** Special analytical rules for policy-driven varieties, overriding the default three-layer weighting.

**Trigger:** Silicon metal oversupply misread — applied the standard cost-floor framework to a variety where government capacity mandates matter more than production costs.

---

## The Pattern · 规律

Every version bump follows the same arc:

```
1. Error occurs (real analysis, real money on the line)
       │
2. Root cause identified (not "I was wrong" but "the framework was blind")
       │
3. Rule added to framework (not a general principle — a specific check)
       │
4. Rule tested against historical cases (does it catch past errors too?)
       │
5. New version deployed (old version archived, not deleted)
```

> **The framework doesn't get smarter by reading. It gets smarter by being wrong and refusing to be wrong the same way twice.**

---

## Current Limitations · 当前局限

| Gap | Status |
|-----|--------|
| Regime-switching model (deficit ↔ surplus transition) | Not yet formalized |
| Cross-commodity spread framework | Early stage |
| Geopolitical risk premium quantification | Qualitative only |
| Real-time sentiment integration | RSS feed processing, no NLP scoring |

These are not bugs — they're the roadmap. Each will be addressed when the agent makes a mistake that traces back to one of these gaps.

---

> **Next:** [Knowledge Taxonomy](knowledge-taxonomy.md) — how the agent decides what to remember
