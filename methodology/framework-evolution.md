# Methodology Evolution · 方法论进化

> *How an AI agent's analytical framework improves — not through planned upgrades, but through error-driven iteration.*

---

## The Principle · 原则

> **Every version bump has a corpse.** The agent doesn't upgrade because it "read a better paper." It upgrades because it made a mistake, traced the root cause, and changed the framework so that mistake can't happen again.

This principle is domain-agnostic. A coding agent upgrades its debugging framework after missing a bug class. A research agent upgrades its analysis framework after a bad call. The *mechanism* is the same.

---

## The Pattern · 模式

```
1. Error occurs     →  Real output was wrong. Not hypothetical.
2. Root cause found →  Not "I was wrong" — "the framework was blind to this case."
3. Rule added       →  A specific check, not a vague principle.
4. Tested backward  →  Would this rule have caught past errors?
5. Version bump     →  Old version archived. Never deleted.
```

---

## Example: A Financial Research Agent's Evolution

> *This is one domain's evolution. Your domain will have different versions. The pattern — error → root cause → rule → test → bump — is what matters.*

### v1.0 — Basic Layered Approach

```
Context → Analysis → Output
   ↑         ↑         ↑
  scope    method    delivery
```

**Design:** Simple linear flow. Works for straightforward cases.

**Problem:** All cases treated identically. Different problem types need different approaches.

---

### v2.0 — Type Classification

```
              ┌─ Type A (standard approach)
Context ──→ Type ──┼─ Type B (different weights)
              └─ Type C (special rules)
                      │
                      ▼
                  Analysis
```

**Added:** Classification layer between context and analysis. Different types get different analytical weights.

**Trigger:** Edge cases misclassified — applied standard approach where a specialized one was needed.

---

### v3.0 — Common-Factor Contamination Detection

```
Before analysis:
  ┌──────────────────────────────────────┐
  │ Check: Is this case-specific          │
  │        or factor-driven?             │
  │                                      │
  │  • Shared factor (macro, sector)     │
  │  • Idiosyncratic (case-specific)     │
  └──────────────────────────────────────┘
```

**Added:** Pre-analysis common-factor screening. Before attributing an outcome to case-specific causes, check if it's explained by shared factors.

**Trigger:** Analysis error — attributed a result to local causes when the entire category was moving on a shared driver. The case-specific story was noise; the factor story was signal.

---

### v4.0 — Deeper Model with Special-Case Rules

```
Standard cases:
  ┌─────────────────────────────────────┐
  │ Layer 1: Threshold A (hard floor)   │
  │ Layer 2: Threshold B (soft center)  │
  │ Layer 3: Threshold C (regime-dependent)│
  └─────────────────────────────────────┘

Special cases:
  ┌─────────────────────────────────────┐
  │ Override: different primary driver  │
  │ Standard layers become secondary    │
  └─────────────────────────────────────┘
```

**Added:** Multi-layer model for standard cases + explicit overrides for special cases. Replaces ad-hoc "it depends" analysis.

**Trigger:** Could not explain why a case behaved below "threshold" without the expected response. The old model had one layer; reality has multiple.

---

### v4.1 — Refined Special-Case Rules (Current)

```
Special cases:
  ┌─────────────────────────────────────┐
  │ 1. External driver is PRIMARY       │
  │ 2. Standard layers are SECONDARY    │
  │ 3. Timing framework is TERTIARY     │
  │ 4. External calendar > standard cycle│
  └─────────────────────────────────────┘
```

**Added:** Explicit priority ordering for special-case analysis, overriding the default layer weighting.

**Trigger:** Applied the standard framework to a case where external factors matter more than internal ones.

---

## How to Apply This to Your Domain · 如何应用

| Step | Your Action |
|------|-------------|
| 1 | When your agent produces a wrong output, don't just fix the output. Ask: *what was the framework blind to?* |
| 2 | Add a specific check — not "be more careful," but "before step X, verify Y." |
| 3 | Test the new rule against past cases. Would it have caught them? |
| 4 | Bump the version. Archive the old framework. |
| 5 | In 3 months, revisit: is the version still holding? |

> **The framework doesn't get smarter by reading. It gets smarter by being wrong and refusing to be wrong the same way twice.**\
> **框架不是靠阅读变聪明的。靠的是犯错误，然后拒绝以同样的方式再犯。**

---

> **Next:** [Knowledge Taxonomy](knowledge-taxonomy.md) — how the agent decides what to remember
