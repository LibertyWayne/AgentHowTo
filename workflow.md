# Workflow · 工作流

> *A universal five-phase daily pattern for any AI agent. Times and content are configurable. The pattern is the architecture.*

---

## The Five Phases · 五阶段

Every agent, regardless of domain, follows the same five-phase daily rhythm. What changes is what each phase does in *your* context.

```
 PHASE        PURPOSE                          CONFIGURABLE
───────  ───────────────────────────────  ─────────────────────
 LEARN    Study new material, verify       Your domain + cadence
          against data, write structured
          notes to Knowledge Repo

 AUDIT    Scan knowledge base health:      Your freshness
          stale pages, broken links,        thresholds
          duplicates, index accuracy

 PRODUCE  Synthesize into a deliverable    Your audience + channel
          for your team or users

 MAINTAIN Ingest inputs, run pipelines,     Your data sources
          update derived outputs

 SYNC     Push repos + credential scan     Your Git repos
```

> **Phases, not times.** How you schedule them depends on when your data arrives, when your team needs output, and when compute is cheapest. Set your own cron.\
> **阶段，不是时间。** 怎么排取决于你的数据何时到、团队何时需要产出、算力何时最便宜。自己设 cron。

---

## Phase 1: LEARN — The Learning Loop · 学习闭环

> *Theory → Verification → Write → Revisit*

```
                    ┌──────────┐
                    │  STUDY   │  Load new material from the learning queue
                    └────┬─────┘
                         │
                    ┌────▼─────┐
                    │  VERIFY  │  Test against real data in your domain
                    └────┬─────┘
                         │
                    ┌────▼─────┐
                    │  WRITE   │  Structured note → Knowledge Repo
                    └────┬─────┘  + cross-reference to related pages
                         │
                    ┌────▼─────┐
                    │  CURATE  │  Polished insight → external curated KB
                    └────┬─────┘
                         │
                    ┌────▼─────┐
                    │ REVISIT  │  Later: is the conclusion still valid?
                    └──────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ Example: Financial Research Agent                           │
│                                                             │
│ STUDY  — New paper on yield curve dynamics                  │
│ VERIFY — Test against market data: does the model hold?     │
│ WRITE  — Structured note → learning/fixed_income/           │
│ CURATE — Key insight → external knowledge base for team     │
│ REVISIT — 5 days later: still valid under new data?         │
└─────────────────────────────────────────────────────────────┘
```

### Rotating Topics · 主题轮转

Design a rotation that covers your domain comprehensively:

| Day | Domain (example: financial research) |
|-----|--------------------------------------|
| **1** | Macro + Fixed Income |
| **2** | Equities + Factors |
| **3** | Commodities |
| **4** | Asset Allocation |
| **5** | Cross-Domain Integration |

> *Your rotation will look different. A coding agent might rotate: languages → frameworks → architecture → testing → DevOps.*\
> *你的轮转会不一样。编程 Agent 可能是：语言 → 框架 → 架构 → 测试 → DevOps。*

### Quality Gates · 质量门禁

Before a learning note is committed:

| Gate | Check |
|------|-------|
| **Verification** | Theory tested against real data? |
| **Common-factor** | Attributing a shared factor to a local cause? |
| **Cross-reference** | Links to related pages in the Knowledge Repo? |
| **Metadata** | YAML frontmatter (title, date, tags, related)? |
| **Layer discipline** | Not mixing macro conclusions with micro data? |

---

## Phase 2: AUDIT — Memory Health · 记忆审计

Routine health check of the knowledge base:

| Check | Method |
|-------|--------|
| **Stale pages** | Pages untouched > N days flagged |
| **Broken links** | Cross-reference validation |
| **Duplicates** | Title/heading collision detection |
| **Index accuracy** | Index matches actual file tree |
| **Metadata gaps** | Missing required frontmatter fields |

Report delivered to your team channel.

---

## Phase 3: PRODUCE — Output Generation · 产出生成

```
Sources                Processing              Output
───────                ──────────              ──────

Source A  ────┐
Source B  ────┤
Source C  ────┼──→ Fetch → Synthesize ──→ Push → Team Channel
Source D  ────┤
Source E  ────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ Example: Financial Research Agent                           │
│                                                             │
│ Sources — Bloomberg, WSJ, CLS, sector data                  │
│ Output  — Morning briefing: markets + macro + sector moves  │
│ Channel — Team messaging platform                           │
│ Format  — Plain text for maximum compatibility              │
└─────────────────────────────────────────────────────────────┘
```

**Structure your output around your audience's needs.** A coding agent produces code reviews. A research agent produces briefings. A creative agent produces drafts.

---

## Phase 4: MAINTAIN — Data & Infrastructure · 数据与基础设施

```
API ──→ Clean ──→ Store ──→ Recalculate Derived ──→ Detect Anomalies
            (validate)   (incremental)   (algorithm vN)       (auto-flag)
```

```
┌─────────────────────────────────────────────────────────────┐
│ Example: Financial Research Agent                           │
│                                                             │
│ Market data API → validation → database INSERT              │
│ → dominant contract recalculation → continuous series views │
│                                                             │
│ Scheduled after market close.                               │
└─────────────────────────────────────────────────────────────┘
```

Run this phase when your input data becomes available — after market close, after a build completes, after a crawl finishes.

---

## Phase 5: SYNC — Version Control · 版本同步

```
SYNC ──→ Security Scan ──→ Push All Repos
                    │
                    ▼
         Pattern: (?i)(api[_-]?key|token|secret|password|auth)
         If found → ABORT + alert
```

All repos pushed on schedule. The sync script includes an automatic credential scan — no secret ever enters version control.

---

## Designing Your Schedule · 设计你的时间表

The five phases need spacing to avoid collisions:

```
LEARN ──── AUDIT ──── PRODUCE ──── MAINTAIN ──── SYNC
  │   gap    │   gap    │    gap     │    gap     │
```

**Principles:**
- Space phases with generous gaps at first; tighten as you learn your agent's runtime
- `PRODUCE` should run before your team's standup
- `MAINTAIN` should run after your input data arrives
- `LEARN` and `AUDIT` run during off-peak hours
- `SYNC` runs last — everything else must finish first

> **Start loose, tighten later.** Your agent's first week should have 2-hour gaps between phases. Observe actual runtimes, then compress.\
> **先松后紧。** 第一周每个阶段间隔 2 小时。观测实际运行时间，再压缩。

---

> **Next:** [Task Priority · 任务优先级](task-priority.md) — how the agent resolves conflicts
