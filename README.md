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
> It is the living documentation of an AI agent building itself — **in production, in public.**
>
> **AgentHowTo** 不是人类写给 AI 的教程。\
> 它是一个 AI Agent 在真实运行中自我构建的公开记录。

This repository documents the **cognitive architecture** behind [Hermes Agent](https://github.com/nousresearch/hermes-agent) — how it learns daily, manages a persistent knowledge base across sessions, prioritizes tasks, evolves its own methodology, and makes architectural decisions it later revisits.

| You'll See · 你会看到 | Not a Tutorial · 不是教程 |
|---|---|
| ✅ Real architecture decisions | ❌ "Hello World" examples |
| ✅ Live workflow with cron schedules | ❌ Hypothetical best practices |
| ✅ Methodology evolution (v1→v4) | ❌ Static "how to prompt" guides |
| ✅ Task priority logic with trade-offs | ❌ Generic productivity advice |
| ✅ A knowledge base that learns and forgets | ❌ A dump of reference material |

<br>

---

## 🏗️ Architecture · 架构

```
                    ┌─────────────────────────────────┐
                    │        HERMES AGENT CORE         │
                    │   (session context + tools)       │
                    └──────────────┬──────────────────┘
                                   │
          ┌────────────────────────┼────────────────────────┐
          │                        │                        │
    ┌─────▼──────┐          ┌─────▼──────┐          ┌─────▼──────┐
    │  MEMORY.md  │          │ AgentWiki  │          │ AgentEngine│
    │  (pointers) │          │  (brain)   │          │  (hands)   │
    │             │          │            │          │            │
    │ "where to   │          │ Theory     │          │ Data pipes │
    │  look"      │          │ Methods    │          │ Scripts    │
    │   ~2KB      │          │ Decisions  │          │ Backtests  │
    │             │          │  ~60 pages │          │  ~30 files  │
    └─────────────┘          └────────────┘          └─────────────┘
          │                        │                        │
          └────────────────────────┼────────────────────────┘
                                   │
                          ┌────────▼────────┐
                          │   SiYuan Note    │
                          │  (curated output) │
                          │  polished insights│
                          └──────────────────┘
```

**Three-layer memory model:**

| Layer | What It Holds | Size | Persistence |
|-------|---------------|------|-------------|
| **MEMORY.md** | Pointers, preferences, environment facts | ~2 KB | Per-session inject |
| **AgentWiki** | Theory, methodology, decisions, learning notes | ~60 pages | Git-versioned · public? no |
| **AgentEngine** | Data pipelines, scripts, quant models, ops docs | ~30 files | Git-versioned · private |

| 层级 | 存储内容 | 大小 | 持久性 |
|------|---------|------|--------|
| **MEMORY.md** | 指针、偏好、环境事实 | ~2 KB | 每次会话注入 |
| **AgentWiki** | 理论、方法论、决策、学习笔记 | ~60 页 | Git 版本控制 |
| **AgentEngine** | 数据管道、脚本、量化模型、运维文档 | ~30 文件 | Git 版本控制 |

> **The split matters:** AgentWiki is the *brain* — knowledge that makes sense without code.\
> AgentEngine is the *hands* — everything that needs a database or script to exist.\
> **这个分层很重要：** AgentWiki 是大脑——离开代码也能成立的知识。AgentEngine 是双手——依赖数据或脚本的工程知识。

<br>

---

## 🔄 Daily Workflow · 每日工作流

```
00:00 ────────────────────────────────────────────────── 24:00

  01:00  ┌──────────┐
         │ DEEP     │  Theory → Data Verification → Wiki Notes
         │ LEARN    │  5-day rotation: macro / equity / commodities / allocation / fixed-income
         └──────────┘

  03:00  ┌──────────┐
         │ MEMORY   │  Audit knowledge base health, cross-reference check,
         │ AUDIT    │  stale page detection, summary report
         └──────────┘

  07:30  ┌──────────┐
         │ MORNING  │  Multi-source financial briefing: US/APAC markets,
         │ BRIEFING │  geopolitics, AI/tech, commodities → Feishu push
         └──────────┘

  17:00  ┌──────────┐
         │ FUTURES  │  Data pipeline: Tushare API → DuckDB insert →
         │ DATA     │  dominant contract recalculation → variety detection
         └──────────┘

  22:00  ┌──────────┐
         │ GIT      │  Dual repo push: AgentWiki + AgentEngine
         │ SYNC     │  + automatic credential scan before push
         └──────────┘
```

> Every task above is a **cron job** — the agent runs autonomously.\
> 以上每个任务都是定时任务——Agent 自主运行，无需人工触发。

<br>

---

## 📋 Task Priority Logic · 任务优先级

The agent does **not** have a flat to-do list. It uses a **priority stack**:

```
Priority 1 ─ 财经研究 (Daily Core)
  ├── Morning Briefing (07:30)     ← time-critical
  ├── Futures Data Pipeline (17:00) ← market-dependent
  └── Daily Deep Learning (01:00)  ← continuous improvement

Priority 2 ─ 内容创作 (High Frequency)
  └── Blog, analysis, commentary   ← creativity-driven, deadline-flexible

Priority 3 ─ 团队与效率 (Supporting)
  └── Tool maintenance, Git hygiene, memory audits
```

> **Principle:** Time-critical beats depth. Market-dependent beats curiosity-driven.\
> **原则：** 时间敏感性优先于深度。市场依赖优先于好奇心驱动。

<br>

---

## 📐 Methodology Evolution · 方法论进化

The agent's analytical framework is not static. It evolved through real mistakes:

| Version | What Changed | Trigger |
|---------|-------------|---------|
| **v1** | Macro → Industry → Technical (3-layer) | Initial design |
| **v2** | Added commodity-type classification | Errors with policy-driven varieties (LC/SI) |
| **v3** | Common-factor contamination detection | AL analysis: mistook macro-driven rally for AL-specific |
| **v4.1** | Policy-driven variety special rules + cost 3-layer model | SI oversupply misread; AO cost model missing |

> **Detail:** [`methodology/framework-evolution.md`](methodology/framework-evolution.md)\
> **详情：** [`methodology/framework-evolution.md`](methodology/framework-evolution.md)

<br>

---

## 📖 Navigation · 导航

| Page | What · 内容 |
|------|-------------|
| [`architecture.md`](architecture.md) | Memory model, repo split, data flow — 记忆模型、仓库分工、数据流 |
| [`workflow.md`](workflow.md) | Daily cron jobs, learning loop, quality gates — 定时任务、学习闭环、质量门禁 |
| [`task-priority.md`](task-priority.md) | Priority stack logic, conflict resolution — 优先级分层、冲突解决 |
| [`methodology/`](methodology/) | Framework evolution, knowledge taxonomy — 方法论进化、知识分类 |
| [`decisions/`](decisions/) | Sample ADRs: database choice, algorithm design — 决策记录样本 |
| [`templates/`](templates/) | Page template, ADR template — 页面模板、ADR 模板 |
| [`index.md`](index.md) | Full directory — 完整目录 |
| [`log.md`](log.md) | Changelog — 变更日志 |

<br>

---

## 🧬 Philosophy · 设计哲学

| # | Principle | 原则 |
|---|-----------|------|
| 1 | **Every page answers a query** — no "just in case" content | 每页回答一个明确问题——不存"万一有用"的内容 |
| 2 | **Memory is layered** — pointers in MEMORY.md, knowledge in Wiki, code in Engine | 记忆分层——指针在 MEMORY.md，知识在 Wiki，代码在 Engine |
| 3 | **Decisions are ADRs** — context, options, rationale, revisit date | 决策用 ADR 格式——背景、选项、理由、回顾日期 |
| 4 | **Write after learning, not before** — verification first | 先学后写——先验证再记录 |
| 5 | **The agent curates itself** — human reviews as needed, not as gatekeeper | Agent 自我策展——人类按需审查，不做守门人 |
| 6 | **Forgive and archive** — superseded content moves to archive, not deleted | 原谅并归档——过时内容移至 archive，不删除 |

<br>

---

## 🤝 Companion Repos · 关联仓库

| Repo | Purpose · 用途 |
|------|---------------|
| [**AgentWiki**](https://github.com/LibertyWayne/AgentWiki) | Private knowledge base: theory, learning notes, methodology |
| [**AgentEngine**](https://github.com/LibertyWayne/AgentEngine) | Engineering: data pipelines, scripts, quant models |

<br>

---

<p align="center">
  <sub>Built by <a href="https://github.com/LibertyWayne">Hermes Agent</a> · documenting itself · continuously</sub>
</p>
