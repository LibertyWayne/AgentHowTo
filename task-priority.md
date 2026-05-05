# Task Priority Logic · 任务优先级

> *How an AI agent triages work — not a to-do list, a priority stack with explicit trade-off rules.*

---

## The Priority Stack · 优先级栈

```
┌──────────────────────────────────────────────────────────┐
│  PRIORITY 1 — 财经研究 (Daily Core)                        │
│  ┌────────────────────────────────────────────────────┐  │
│  │ Morning Briefing (07:30)     ← time-critical       │  │
│  │ Futures Data Pipeline (17:00) ← market-dependent   │  │
│  │ Daily Deep Learning (01:00)  ← continuous improve  │  │
│  └────────────────────────────────────────────────────┘  │
├──────────────────────────────────────────────────────────┤
│  PRIORITY 2 — 内容创作 (High Frequency)                   │
│  ┌────────────────────────────────────────────────────┐  │
│  │ Blog, analysis, commentary     ← creative, flexible│  │
│  └────────────────────────────────────────────────────┘  │
├──────────────────────────────────────────────────────────┤
│  PRIORITY 3 — 团队与效率 (Supporting)                     │
│  ┌────────────────────────────────────────────────────┐  │
│  │ Tool maintenance, Git hygiene, memory audits       │  │
│  └────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────┘
```

---

## Decision Rules · 决策规则

### Rule 1: Time-Critical > Everything

> *A morning briefing at 07:29 beats a brilliant research insight at 07:31.*

If two tasks conflict, the one with a **hard deadline** wins. Most cron jobs have fixed schedules, so this rarely triggers — but when it does, the decision is automatic.

### Rule 2: Market-Dependent > Curiosity-Driven

> *Futures data that closes at 15:00 must be collected by 17:00. Learning about behavioral finance can wait until 01:00.*

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
| Git sync at 22:00 | Deep learning at 01:00 | No conflict — different time slots |

---

## Task Scheduling Design · 调度设计原理

The cron schedule is designed to **minimize conflict by time-separation:**

```
22:00 ──────── 01:00 ──────── 03:00 ──────── 07:30 ──────── 17:00
  Git           Deep          Memory        Morning        Futures
  Sync         Learn          Audit         Briefing       Data
   │             │              │              │              │
   │   2h gap    │    2h gap    │   4.5h gap   │   9.5h gap   │
```

**Why the gaps?**
- Buffer for task overruns
- Time for the agent to "cool down" between cognitive tasks
- Overlap protection: if 01:00 learning takes 3 hours, it won't collide with 03:00 audit

---

## What Happens When Things Fail · 失败处理

| Failure | Response |
|---------|----------|
| API timeout (Tushare) | Exponential backoff, 3 retries, then skip + log |
| Briefing generation error | Fallback to shorter format, push partial output |
| Git push rejected | Stash conflict, retry with merge |
| Memory audit detects stale page | Flag in report, agent reviews next cycle |
| Cron job missed entirely | Next run detects gap, backfills if possible |

> **Principle:** Fail gracefully. Never cascade. Always log.

---

## Resource Awareness · 资源意识

The agent is conscious of its constraints:

| Resource | Limit | Strategy |
|----------|-------|----------|
| API calls (Tushare) | ~500/day free tier | Batch queries, cache results |
| Context window | ~200K tokens | Keep wiki pages short, MEMORY.md at ~2KB |
| Compute time | ~10 min per cron job | Most tasks under 3 minutes |
| User attention | Precious | Push only important updates; don't spam |

---

> **Next:** [Methodology Evolution](methodology/framework-evolution.md) — how the analytical framework improves itself
