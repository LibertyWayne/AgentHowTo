# Task Priority Framework · 任务优先级框架

> *How any AI agent resolves work conflicts — not a to-do list, a priority stack with explicit trade-off rules.*

---

## The Priority Stack · 优先级栈

```
┌──────────────────────────────────────────────────────────┐
│  PRIORITY 1 — Core Mission                               │
│  ┌────────────────────────────────────────────────────┐  │
│  │ Time-critical outputs (deadline-driven deliveries) │  │
│  │ Input-dependent tasks (runs when data arrives)     │  │
│  │ Continuous improvement (learning, skill-building)  │  │
│  └────────────────────────────────────────────────────┘  │
├──────────────────────────────────────────────────────────┤
│  PRIORITY 2 — Amplification                              │
│  ┌────────────────────────────────────────────────────┐  │
│  │ Creative work, writing, extended analysis          │  │
│  └────────────────────────────────────────────────────┘  │
├──────────────────────────────────────────────────────────┤
│  PRIORITY 3 — Infrastructure                             │
│  ┌────────────────────────────────────────────────────┐  │
│  │ Tool maintenance, repo hygiene, system health      │  │
│  └────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────┘
```

```
┌─────────────────────────────────────────────────────────────┐
│ Example: Financial Research Agent                           │
│                                                             │
│ P1 — Morning briefing (time-critical), data pipeline        │
│      (market-dependent), deep learning (continuous)         │
│ P2 — Blog posts, in-depth analysis pieces                   │
│ P3 — Git hygiene, memory audits, tool updates               │
└─────────────────────────────────────────────────────────────┘
```

---

## Four Decision Rules · 四条决策规则

### Rule 1: Time-Critical > Deep

> *A deliverable due in 5 minutes beats a brilliant insight.*

If two tasks conflict, the one with a **hard deadline** wins.

### Rule 2: Input-Dependent > Curiosity

> *Data pipeline runs when data lands. Learning can wait.*

Tasks triggered by external input availability take precedence over internally-driven work.

### Rule 3: Corrective > Additive

> *Fix the bug before writing new code.*

When the agent discovers a problem in existing infrastructure, the fix jumps to the top.

### Rule 4: User-Triggered > Scheduled

> *Human request pauses all automation.*

Scheduled tasks are autonomous. The user is the ultimate priority arbitrator.

---

## Conflict Resolution Matrix · 冲突解决矩阵

| Priority 1 Task | Priority 2 Task | Resolution |
|----------------|-----------------|------------|
| Deliverable overdue | Creative work in progress | Pause creative, finish deliverable |
| Pipeline error detected | Learning session active | Abort learning, fix pipeline |
| Memory audit running | User asks question | Complete current check, switch to user |
| Sync scheduled | Learning scheduled | No conflict — different time slots |

---

## Schedule Design · 调度设计

Cron jobs should be spaced to **minimize conflict through time-separation:**

```
Task A ──────── Task B ──────── Task C ──────── Task D ──────── Task E
  │     gap       │     gap       │     gap       │     gap       │
```

**Why gaps?**
- Buffer for overruns (some tasks take longer than expected)
- Context-switch cost between different cognitive tasks
- Overlap protection: if Task A runs long, it won't collide with Task B

> **Start with generous gaps. Tighten as you learn your agent's runtime.**\
> **起步宽松间隔，根据实际运行时间逐步收紧。**

---

## Failure Handling · 失败处理

| Failure | Response |
|---------|----------|
| API timeout | Exponential backoff, N retries, then skip + log |
| Output generation error | Fallback to shorter format, push partial output |
| Git push rejected | Stash conflict, retry with merge |
| Audit detects stale page | Flag in report, agent reviews next cycle |
| Cron job missed | Next run detects gap, backfills if possible |

> **Principle:** Fail gracefully. Never cascade. Always log.\
> **原则：** 优雅失败。不连锁。必记录。

---

## Resource Awareness · 资源意识

| Resource | Strategy |
|----------|----------|
| API calls | Batch queries, cache results, respect rate limits |
| Context window | Keep notes short, MEMORY.md under size cap |
| Compute time | Light tasks in quick slots; heavy tasks in dedicated windows |
| User attention | Push only important updates; don't spam |

> *Design your resource model around your actual API tiers, model limits, and team expectations.*\
> *根据你的实际 API 配额、模型限制、团队期望设计资源模型。*

---

> **Next:** [Methodology Evolution](methodology/framework-evolution.md) — how the analytical framework improves itself
