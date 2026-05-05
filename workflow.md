# Workflow · 工作流

> *How an AI agent structures its day — cron-based automation, learning loops, and quality gates. All times are configurable; the pattern is the design pattern.*

---

## The Daily Rhythm · 每日节奏

```
 TIME BLOCK              TASK                    TYPE              FREQUENCY
───────────  ──────────────────────────────  ────────────────  ───────────
 Early AM    Deep Learning                   Research          Daily/Mon–Fri
 Morning     Memory Audit                    Maintenance       Daily
 Pre-Market  Intelligence Briefing            Production        Mon–Fri
 Post-Market Data Pipeline                   Production        Mon–Fri
 Night       Git Sync + Security Scan        Maintenance       Daily
```

> **Set the times to match your market hours, your team's schedule, and your data availability.**\
> **根据你的市场时间、团队日程、数据窗口设置具体时间——这个顺序和间隔才是设计重点。**

---

## Deep Learning Loop · 深度学习闭环

> *The heart of the system: theory → verification → write → revisit.*

```
                    ┌──────────┐
                    │  THEORY   │  Load a new concept from the learning queue
                    └────┬─────┘
                         │
                    ┌────▼─────┐
                    │  VERIFY   │  Test against real market data
                    └────┬─────┘
                         │
                    ┌────▼─────┐
                    │  WRITE    │  Structured note → Knowledge Repo learning/
                    └────┬─────┘  + cross-reference to related pages
                         │
                    ┌────▼─────┐
                    │  CURATE   │  Polished insight → external curated KB
                    └────┬─────┘
                         │
                    ┌────▼─────┐
                    │  REVISIT  │  N days later: is the conclusion still valid?
                    └──────────┘
```

### Topic Rotation · 主题轮转

The agent follows a structured rotation to cover your domain comprehensively. Design your own rotation based on your research scope:

| Day | Domain (example) | Topics |
|-----|-----------------|--------|
| **1** | Macro + Fixed Income | GDP, inflation, yield curves, credit cycles |
| **2** | Equities + Factors | Valuation models, factor analysis, style rotation |
| **3** | Commodities | Cost models, term structure, basis trading |
| **4** | Asset Allocation | Risk parity, tactical shifts, rebalancing |
| **5** | Cross-Domain | Combined frameworks, multi-asset signals |

> *This is an example rotation for a financial research agent. Adapt the domains and cadence to your field.*\
> *这是一个金融研究 Agent 的示例轮转。根据你的领域调整主题和频率。*

### Quality Gates · 质量门禁

Before a learning note is committed, it must pass:

| Gate | Check |
|------|-------|
| **Data verification** | Is the theory tested against real data? |
| **Common-factor check** | Am I attributing a factor-driven move to a case-specific cause? |
| **Cross-reference** | Does this note link to related pages? |
| **Frontmatter** | Does it have YAML metadata (title, date, tags, related)? |
| **No layer-jumping** | Am I mixing macro conclusions with micro data without the intermediate layer? |

---

## Intelligence Briefing · 情报简报

> Production output — the agent's most visible daily deliverable.

```
Sources                    Processing                  Output
───────                    ──────────                  ──────

News API 1  ────┐
News API 2  ────┤
Wire 1       ───┤           ┌───────────────┐          ┌──────────┐
Wire 2       ───┼──→ Fetch →│  Multi-source │──→ Push →│  Team    │
Market Data  ──┤           │  Synthesis    │          │  Channel │
Sector Data  ──┤           └───────────────┘          └──────────┘
Specialty    ──┘
```

**Structure (example):**
1. Overnight markets summary
2. Geopolitics & macro developments
3. Regional market preview
4. Sector & asset class movements
5. Technology & emerging trends

**Format:** Plain text for maximum compatibility · Concise · Delivered before your team's morning standup.

---

## Data Pipeline · 数据管道

```
Market API ──→ Data Cleaning ──→ Database INSERT ──→ Derived Series Recalc ──→ Anomaly Detection
                  (validation)      (incremental)         (algorithm vN)           (auto-flag)
```

Scheduled after your market closes. Details in [Architecture → Data Flow](architecture.md#data-flow--数据流).

---

## Memory Audit · 记忆审计

Routine health check of the knowledge base:

| Check | Method |
|-------|--------|
| **Stale pages** | Pages untouched >N days flagged |
| **Broken links** | Cross-reference validation |
| **Duplicate content** | Title/heading collision detection |
| **Index accuracy** | Index matches actual file tree |
| **Frontmatter completeness** | Missing required fields reported |

Report delivered to your team channel.

---

## Git Hygiene · 版本控制纪律

```
Night ──→ git_sync.sh ──→ Security Scan ──→ Push All Repos
                                     │
                                     ▼
                          Pattern: (?i)(api[_-]?key|token|secret|password|auth)
                          If found → ABORT + alert
```

All repos are pushed on schedule. The sync script includes an automatic credential scan — no API key or token ever enters version control.

---

> **Next:** [Task Priority · 任务优先级](task-priority.md) — how the agent decides what to do when conflicts arise
