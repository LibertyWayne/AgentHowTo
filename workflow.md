# Workflow · 工作流

> *How an AI agent structures its day — autonomous cron jobs, learning loops, and quality gates.*

---

## The Daily Rhythm · 每日节奏

```
 TIME    TASK                    TYPE              FREQUENCY
──────   ─────────────────────   ────────────────  ───────────
 01:00   Daily Deep Learning     Research          Mon–Fri
 03:00   Memory Audit            Maintenance       Daily
 07:30   Morning Briefing        Production        Mon–Fri
 17:00   Futures Data Pipeline   Production        Mon–Fri
 22:00   Git Sync                Maintenance       Daily
```

Every task is a **cron job** — no human trigger required. The agent wakes up, runs the task, produces output, and goes back to waiting.

---

## Deep Learning Loop · 深度学习闭环

> *The heart of the system: theory → verification → write → revisit.*

```
                    ┌──────────┐
                    │  THEORY   │  Load a new concept from the learning queue
                    └────┬─────┘  (e.g., "cost-of-carry model")
                         │
                    ┌────▼─────┐
                    │  VERIFY   │  Test against real market data
                    └────┬─────┘  (e.g., Does carry predict basis? On which varieties?)
                         │
                    ┌────▼─────┐
                    │  WRITE    │  Structured note → AgentWiki learning/
                    └────┬─────┘  + cross-reference to related pages
                         │
                    ┌────▼─────┐
                    │  PUBLISH  │  Curated insight → SiYuan Note
                    └────┬─────┘  (external KB for polished output)
                         │
                    ┌────▼─────┐
                    │  REVISIT  │  5 days later: is the conclusion still valid?
                    └──────────┘  (checks for staleness, updates if needed)
```

### 5-Day Rotation · 五日轮转

The agent doesn't learn random topics. It follows a structured rotation to cover the full investment research landscape:

| Day | Domain | Example Topics |
|-----|--------|---------------|
| **1** | Macroeconomics + Fixed Income | GDP, inflation, yield curves, credit cycles |
| **2** | Equities + Multi-Factor | Valuation models, factor zoo, style rotation |
| **3** | Commodity Futures | Cost models, term structure, basis trading |
| **4** | Asset Allocation | Risk parity, tactical allocation, rebalancing |
| **5** | Cross-Domain Integration | Combined frameworks, multi-asset signals |

### Quality Gates · 质量门禁

Before a learning note is committed, it must pass:

| Gate | Check |
|------|-------|
| **Data verification** | Is the theory tested against real market data? |
| **Common-factor check** | Am I attributing a factor-driven move to a variety-specific cause? |
| **Cross-reference** | Does this note link to related pages in the wiki? |
| **Frontmatter** | Does it have YAML metadata (title, date, tags, related)? |
| **No layer-jumping** | Am I mixing macro conclusions with micro data without the intermediate layer? |

---

## Morning Briefing · 财经早报

> Production output — the agent's most visible daily deliverable.

```
Sources                    Processing                  Output
───────                    ──────────                  ──────

Bloomberg  ────┐
WSJ         ───┤
Reuters     ───┤           ┌───────────────┐          ┌──────────┐
CLS         ───┼──→ Fetch →│  Multi-source │──→ Push →│  Feishu  │
WSCN        ───┤           │  Synthesis    │          │  Channel │
EastMoney   ───┤           └───────────────┘          └──────────┘
SunSirs     ───┘
```

**Structure:**
1. US Markets (overnight close + key movers)
2. Geopolitics & Macro (Fed, PBOC, major policies)
3. APAC Markets (Japan, HK, China pre-market)
4. A-Share & Commodities (sector rotation, key futures)
5. AI & Technology (model releases, regulatory updates)

**Format:** Plain text (Feishu compatibility) · ~800 words · delivered 07:30 CST

---

## Futures Data Pipeline · 期货数据管道

```
Tushare API ──→ Data Cleaning ──→ DuckDB INSERT ──→ Dominant Map Recalc ──→ New Variety Detection
     (proxy)        (validation)      (incremental)       (v6: vol + 115%)        (auto-add)
```

Details in [Architecture → Data Flow](architecture.md#data-flow--数据流).

---

## Memory Audit · 记忆审计

Daily health check of the knowledge base:

| Check | Method |
|-------|--------|
| **Stale pages** | Pages untouched >7 days flagged |
| **Broken links** | Cross-reference validation |
| **Duplicate content** | Title/heading collision detection |
| **Index accuracy** | Index.md matches actual file tree |
| **Frontmatter completeness** | Missing required fields reported |

Report delivered as Feishu message at 03:00 CST daily.

---

## Git Hygiene · 版本控制纪律

```
22:00 ──→ git_sync.sh ──→ Security Scan ──→ Push Both Repos
                                     │
                                     ▼
                          Pattern: (?i)(api[_-]?key|token|secret|password|auth)
                          If found → ABORT + alert
```

Both AgentWiki and AgentEngine are pushed nightly. The sync script includes an automatic credential scan — no API key or token ever enters GitHub.

---

> **Next:** [Task Priority · 任务优先级](task-priority.md) — how the agent decides what to do when conflicts arise
