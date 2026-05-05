# Knowledge Taxonomy · 知识分类法

> *How the agent decides what to remember, where to store it, and when to let it go.*

---

## The Core Question · 核心问题

After every learning session, the agent faces a decision: **should this be remembered?**

| Question | If Yes | If No |
|----------|--------|-------|
| Can it prevent a future mistake? | Write to Wiki | Discard |
| Does it need code or data to be useful? | Write to Engine | Wiki |
| Is it already covered elsewhere? | Cross-reference | New page |
| Will it still matter in 3 months? | Permanent note | Journal entry |
| Can it be verified with data? | Verified note | Mark as hypothesis |

---

## The Taxonomy · 分类体系

```
New Knowledge
     │
     ├──→ Theory (verified with data) ──→ learning/
     │         e.g., "Cost-of-carry: tested on CU/AL/ZN basis"
     │
     ├──→ Methodology (how to analyze) ──→ methodology/
     │         e.g., "Commodity framework v4.1 decision tree"
     │
     ├──→ Tool Experience (how to use X) ──→ tools/ (in Engine)
     │         e.g., "DuckDB: WINDOW functions vs GROUP BY performance"
     │
     ├──→ Decision (why we chose Y) ──→ decisions/
     │         e.g., "ADR-002: Dominant contract v6 algorithm"
     │
     ├──→ Operational (how to run Z) ──→ docs/ (in Engine)
     │         e.g., "System rebuild guide after server migration"
     │
     └──→ Transient (what happened today) ──→ journal/ (private)
               e.g., "2026-05-05: Fixed credential leak, repo split"
```

---

## Storage Decision Matrix · 存储决策矩阵

| Knowledge Type | Knowledge Repo | Engineering Repo | Journal | Discard |
|---------------|-----------|-------------|---------|---------|
| Economic theory + data test | ✅ | — | — | — |
| Factor model construction | — | ✅ | — | — |
| Analysis methodology | ✅ | — | — | — |
| API quirks & tool patterns | — | ✅ | — | — |
| Architecture decision | ✅ | — | — | — |
| Today's bug fixes | — | — | ✅ | — |
| "Interesting but unverified" | — | — | — | ✅ |
| Duplicate of existing page | — | — | — | ✅ |

---

## When to Archive · 何时归档

Content moves to `archive/` when:

| Trigger | Example |
|---------|---------|
| Framework superseded | v3.0 methodology → archive, v4.0 active |
| Analysis outdated by new data | April copper analysis when May fundamentals changed |
| Initial draft replaced by deep version | IMA knowledge base skim → archive, deep analysis → active |

> **Never delete.** Archive preserves the evolution trail. The agent can revisit old analyses to see if its thinking has improved — or if it's making the same mistake again.

---

## When to Forget · 何时遗忘

Some knowledge should be **actively forgotten:**

| Type | Action |
|------|--------|
| Unverified claims | Remove, don't archive |
| Stale facts (>6 months unverified) | Move to archive with `[STALE]` tag |
| Emotional reactions | Don't record — journal is work log, not diary |

> **The wiki is not a brain dump. It's a curated collection.** Every page must earn its place by answering a question the agent will actually ask again.

---

> **Next:** [Decisions](decisions/sample-adr.md) — an example Architecture Decision Record
