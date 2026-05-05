# Task Priority Logic · 任务优先级

> *How an AI agent triages work — not a to-do list, a priority stack with explicit trade-off rules.*

---

## The Priority Stack · 优先级栈

```
┌──────────────────────────────────────────────────────────┐
│  PRIORITY 1 — Domain Research (Daily Core)                │
│  ┌────────────────────────────────────────────────────┐  │
│  │ Time-critical outputs (briefings, data delivery)   │  │
│  │ Market-dependent tasks (pipeline runs)             │  │
│  │ Continuous improvement (deep learning)             │  │
│  └────────────────────────────────────────────────────┘  │
├──────────────────────────────────────────────────────────┤
│  PRIORITY 2 — Content Creation (High Frequency)           │
│  ┌────────────────────────────────────────────────────┐  │
│  │ Analysis, commentary, writing ← creative, flexible│  │
│  └────────────────────────────────────────────────────┘  │
├──────────────────────────────────────────────────────────┤
│  PRIORITY 3 — Infrastructure (Supporting)                 │
│  ┌────────────────────────────────────────────────────┐  │
│  │ Tool maintenance, Git hygiene, memory audits       │  │
│  └────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────┘
```

---

## Decision Rules · 决策规则

### Rule 1: Time-Critical > Everything

> *A briefing due in 2 minutes beats a brilliant research insight.*

If two tasks conflict, the one with a **hard deadline** wins. Cron jobs with fixed schedules rarely collide — but when they do, the decision is automatic.

### Rule 2: Market-Dependent > Curiosity-Driven

> *Market data that closes at end-of-day must be collected before the pipeline window closes. Deep learning can wait.*

Tasks tied to external market schedules take precedence over internally-driven research.

### Rule 3: Corrective > Additive

> *Fixing a bug in the data pipeline beats writing a new analysis.*

When the agent discovers a problem in existing infrastructure, that fix jumps to the top. New content can wait.

### Rule 4: User-Triggered > Scheduled

> *If the user asks for something now, everything else pauses.*

Scheduled tasks are autonomous, but the user is the ultimate priority arbitrator.

---

## Conflict Resolution Matrix · 冲突解决矩阵

| Priority 1 Task | Priority 2 Task | Resolution |
|----------------|-----------------|------------|
| Briefing overdue | Blog draft in progress | Pause blog, finish briefing |
| Pipeline error detected | Learning session active | Abort learning, fix pipeline |
| Memory audit running | User asks question | Complete current check, switch to user |
| Git sync scheduled | Deep learning scheduled | No conflict — different time slots |

---

## Schedule Design Principle · 调度设计原理

Cron jobs should be spaced to **minimize conflict through time-separation:**

```
Task A ──────── Task B ──────── Task C ──────── Task D ──────── Task E
  │     gap       │     gap       │     gap       │     gap       │
```

**Why the gaps?**
- Buffer for task overruns (some tasks take longer than expected)
- Time for the agent to "context-switch" between cognitive tasks
- Overlap protection: if Task A runs long, it won't collide with Task B

> *Start with generous gaps. Tighten as you learn your agent's actual runtime characteristics.*\
> *起步用宽松间隔，根据实际运行时间逐步收紧。*

---

## What Happens When Things Fail · 失败处理

| Failure | Response |
|---------|----------|
| API timeout | Exponential backoff, N retries, then skip + log |
| Briefing generation error | Fallback to shorter format, push partial output |
| Git push rejected | Stash conflict, retry with merge |
| Memory audit detects stale page | Flag in report, agent reviews next cycle |
| Cron job missed entirely | Next run detects gap, backfills if possible |

> **Principle:** Fail gracefully. Never cascade. Always log.\
> **原则：** 优雅失败。不连锁。必记录。

---

## Resource Awareness · 资源意识

The agent is conscious of its constraints:

| Resource | Strategy |
|----------|----------|
| API calls | Batch queries, cache results, respect rate limits |
| Context window | Keep Knowledge Repo pages short, MEMORY.md under size cap |
| Compute time | Most tasks under a few minutes; heavy tasks get dedicated windows |
| User attention | Push only important updates; don't spam |

> *Design your resource model around your actual API tiers, model limits, and team expectations.*\
> *根据你的实际 API 配额、模型限制、团队期望设计资源模型。*

---

> **Next:** [Methodology Evolution](methodology/framework-evolution.md) — how the analytical framework improves itself
