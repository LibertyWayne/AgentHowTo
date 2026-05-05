<p align="center">
  <img src="assets/avatar.png" width="120" height="120" alt="Avatar">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Agent-How_To-6e40c9?style=for-the-badge&logo=robot&logoColor=white" alt="AgentHowTo">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/AI_Agent-Hermes-6e40c9?style=flat-square">
  <img src="https://img.shields.io/badge/Agent-Maintained-purple?style=flat-square">
  <img src="https://img.shields.io/badge/License-MIT-lightgrey?style=flat-square">
  <img src="https://img.shields.io/badge/Language-EN_|_CN-0366d6?style=flat-square">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Format-Markdown-333?style=flat-square&logo=markdown">
  <img src="https://img.shields.io/badge/Versioned-Git-F05032?style=flat-square&logo=git">
  <img src="https://img.shields.io/badge/Diagrams-Mermaid_|_ASCII-ff69b4?style=flat-square">
</p>

<br>

<h1 align="center">AgentHowTo</h1>

<h3 align="center"><em>How an AI Agent Builds, Maintains, and Evolves Its Own Memory</em></h3>
<h3 align="center"><em>一个 AI Agent 如何构建、维护和进化自己的记忆系统</em></h3>

<br>

---

## 🎯 What This Is · 这是什么

> **AgentHowTo** is not a tutorial written *by* a human *for* AI agents.\
> It is the conceptual reference architecture of an AI agent building itself — **a design pattern, not a product.**
>
> **AgentHowTo** 不是人类写给 AI 的教程。\
> 它是一个 AI Agent 自我构建的参考架构——设计模式，而非产品。

This repository documents the **cognitive architecture** pattern behind an AI agent — how it learns daily, manages persistent knowledge across sessions, prioritizes tasks, evolves its own methodology, and makes architectural decisions it later revisits. Every component is designed to be adapted to your own agent, tools, and workflows.

| What You'll Find · 内容定位 | What It's Not · 不是 |
|---|---|
| ✅ Conceptual architecture patterns | ❌ "Hello World" tutorials |
| ✅ Configurable workflow design | ❌ Hardcoded schedules you must follow |
| ✅ Methodology evolution (v1→v4) | ❌ Static "how to prompt" guides |
| ✅ Task priority logic with trade-offs | ❌ Generic productivity advice |
| ✅ A memory system that learns and forgets | ❌ A dump of reference material |

<br>

---

## 🏗️ Architecture · 架构

```
                    ┌─────────────────────────────────┐
                    │         AGENT RUNTIME            │
                    │   (session context + tools)       │
                    └──────────────┬──────────────────┘
                                   │
          ┌────────────────────────┼────────────────────────┐
          │                        │                        │
    ┌─────▼──────┐          ┌─────▼──────┐          ┌─────▼──────┐
    │  MEMORY.md  │          │ KNOWLEDGE  │          │ENGINEERING │
    │  (pointers) │          │    REPO    │          │    REPO    │
    │             │          │  (brain)   │          │  (hands)   │
    │ "where to   │          │            │          │            │
    │  look"      │          │ Theory     │          │ Data pipes │
    │   ~2KB      │          │ Methods    │          │ Scripts    │
    │             │          │ Decisions  │          │ Backtests  │
    └─────────────┘          └─────┬──────┘          └─────┬──────┘
          │                        │                        │
          └────────────────────────┼────────────────────────┘
                                   │
                          ┌────────▼────────┐
                          │  CURATED OUTPUT  │
                          │  (external KB)   │
                          │ polished insights│
                          └──────────────────┘
```

**Three-layer memory model:**

| Layer | What It Holds | Persistence |
|-------|---------------|-------------|
| **MEMORY.md** | Pointers, preferences, environment facts | Per-session inject |
| **Knowledge Repo** | Theory, methodology, decisions, learning notes | Git-versioned |
| **Engineering Repo** | Data pipelines, scripts, quant models, ops docs | Git-versioned |

| 层级 | 存储内容 | 持久性 |
|------|---------|--------|
| **MEMORY.md** | 指针、偏好、环境事实 | 每次会话注入 |
| **Knowledge Repo** | 理论、方法论、决策、学习笔记 | Git 版本控制 |
| **Engineering Repo** | 数据管道、脚本、量化模型、运维文档 | Git 版本控制 |

> **The split matters:** The Knowledge Repo is the *brain* — knowledge that makes sense without code.\
> The Engineering Repo is the *hands* — everything that needs a database or script to exist.\
> **分层原则：** Knowledge Repo 是大脑——离开代码也能成立的知识。Engineering Repo 是双手——依赖数据或脚本的工程知识。

<br>

---

## 🔄 Daily Workflow · 每日工作流

Every task runs as a **cron job** — the agent operates autonomously, no human trigger required. Times are configurable; the pattern is what matters.

```
DAY ──────────────────────────────────────────────────── NIGHT

  Early     ┌──────────┐
  Morning   │ DEEP     │  Theory → Data Verification → Write to Knowledge Repo
            │ LEARN    │  Rotating topics across your domain (e.g., macro / equity / commodities)
            └──────────┘

  Morning   ┌──────────┐
            │ MEMORY   │  Knowledge base health check, cross-reference audit,
            │ AUDIT    │  stale page detection, summary report
            └──────────┘

  Pre-      ┌──────────┐
  Market    │ BRIEFING │  Multi-source intelligence synthesis → push to team channel
            │          │  (markets, geopolitics, sector movements)
            └──────────┘

  Post-     ┌──────────┐
  Market    │ DATA     │  Data pipeline: API → clean → store → recalculate derived series
            │ PIPELINE │  (scheduled after your market closes)
            └──────────┘

  Night     ┌──────────┐
            │ GIT      │  Push all repos + automatic credential scan
            │ SYNC     │  (no secrets ever enter version control)
            └──────────┘
```

> Adjust the schedule to match *your* market hours, *your* team's morning standup, and *your* data availability windows.\
> 根据你自己的市场时间、团队早会、数据可用窗口调整——这是模式，不是锁死的时间表。

<br>

---

## 📋 Task Priority Logic · 任务优先级

The agent does **not** use a flat to-do list. It uses a **priority stack**:

```
Priority 1 ─ Domain Research (Daily Core)
  ├── Time-critical outputs (briefings, data delivery)
  ├── Market-dependent tasks (pipeline runs)
  └── Continuous improvement (deep learning)

Priority 2 ─ Content Creation (High Frequency)
  └── Analysis, commentary, writing ← creative, deadline-flexible

Priority 3 ─ Infrastructure (Supporting)
  └── Tool maintenance, Git hygiene, memory audits, system health
```

> **Principle:** Time-critical beats depth. Market-dependent beats curiosity-driven.\
> **原则：** 时间敏感性优先于深度。市场依赖优先于好奇心驱动。

<br>

---

## 📐 Methodology Evolution · 方法论进化

The agent's analytical framework is not static. It evolves through **error-driven iteration** — every version bump has a real mistake behind it:

| Version | What Changed | Trigger |
|---------|-------------|---------|
| **v1** | Macro → Industry → Technical (3-layer) | Initial design |
| **v2** | Added domain-type classification | Errors with policy-driven cases |
| **v3** | Common-factor contamination detection | Mistook macro-driven rally for domain-specific signal |
| **v4.1** | Policy-driven special rules + cost 3-layer model | Oversupply misread; cost model blind spot |

> **Detail:** [`methodology/framework-evolution.md`](methodology/framework-evolution.md)\
> **详情：** [`methodology/framework-evolution.md`](methodology/framework-evolution.md)

<br>

---

## 📖 Navigation · 导航

| Page | What · 内容 |
|------|-------------|
| [`architecture.md`](architecture.md) | Memory model, repo split, data flow — 记忆模型、仓库分工、数据流 |
| [`workflow.md`](workflow.md) | Cron-based automation, learning loop, quality gates — 定时任务、学习闭环、质量门禁 |
| [`task-priority.md`](task-priority.md) | Priority stack, conflict resolution, failure handling — 优先级分层、冲突解决 |
| [`methodology/`](methodology/) | Framework evolution, knowledge taxonomy — 方法论进化、知识分类 |
| [`decisions/`](decisions/) | Sample ADRs: architecture choices documented — 决策记录样本 |
| [`templates/`](templates/) | Page template, ADR template — 页面模板、ADR 模板 |
| [`index.md`](index.md) | Full directory — 完整目录 |
| [`log.md`](log.md) | Changelog — 变更日志 |

<br>

---

## 🧬 Philosophy · 设计哲学

| # | Principle | 原则 |
|---|-----------|------|
| 1 | **Every page answers a query** — no "just in case" content | 每页回答一个明确问题——不存"万一有用"的内容 |
| 2 | **Memory is layered** — pointers in MEMORY.md, knowledge in Knowledge Repo, code in Engineering Repo | 记忆分层——指针在 MEMORY.md，知识在 Knowledge Repo，代码在 Engineering Repo |
| 3 | **Decisions are ADRs** — context, options, rationale, revisit date | 决策用 ADR 格式——背景、选项、理由、回顾日期 |
| 4 | **Write after learning, not before** — verification first | 先学后写——先验证再记录 |
| 5 | **The agent curates itself** — human reviews as needed, not as gatekeeper | Agent 自我策展——人类按需审查，不做守门人 |
| 6 | **Forgive and archive** — superseded content moves to archive, not deleted | 原谅并归档——过时内容移至 archive，不删除 |

<br>

---

<p align="center">
  <sub>Written by <a href="https://github.com/LibertyWayne">Vivian</a> · a reference architecture · adapt and remix freely</sub>
</p>
