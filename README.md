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
  <img src="https://img.shields.io/badge/Language-EN_|_中文-0366d6?style=flat-square">
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Format-Markdown-333?style=flat-square&logo=markdown">
  <img src="https://img.shields.io/badge/Versioned-Git-F05032?style=flat-square&logo=git">
  <img src="https://img.shields.io/badge/Type-Reference_Architecture-6e40c9?style=flat-square">
</p>

<br>

<p align="center">
  <a href="#english">🇬🇧 English</a> &nbsp;·&nbsp;
  <a href="#中文">🇨🇳 中文</a>
</p>

<br>

<h1 align="center">AgentHowTo</h1>

<h3 align="center"><em>A Universal Cognitive Architecture for AI Agents</em></h3>

<br>

---

<span id="english"></span>

## English

---

### 🎯 What This Is

**AgentHowTo** is a domain-agnostic reference architecture for how an AI agent manages long-term memory, learns continuously, prioritizes work, and evolves its own methodology — across sessions, without fine-tuning, using only Markdown and Git.

This is not a tutorial for a specific use case. It is the **structural pattern** that any domain-specific agent can adopt. A financial research agent? Plug in market data. A coding agent? Plug in code review cycles. A creative assistant? Plug in content iterations.

| You'll Find | You Won't Find |
|---|---|
| ✅ Memory architecture (3-layer model) | ❌ Domain-specific implementation details |
| ✅ Workflow pattern (learn → audit → produce → maintain) | ❌ Hardcoded schedules or tools |
| ✅ Task priority framework (4 decision rules) | ❌ "Best prompts for X" |
| ✅ Methodology evolution (error-driven iteration) | ❌ Specific analysis techniques |
| ✅ Decision records (ADR format) | ❌ Finished products or outputs |

> **Concrete example:** The sidebar boxes show how a financial research agent instantiates these patterns. The architecture itself works for any domain.

---

### 🏗️ Architecture

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
    │  look"      │          │ Theory     │          │ Code       │
    │   ~2KB      │          │ Methods    │          │ Data       │
    │             │          │ Decisions  │          │ Config     │
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

| Layer | What It Holds | Persistence | Purpose |
|-------|---------------|-------------|---------|
| **MEMORY.md** | Pointers, preferences, environment facts | ~2 KB per session | Tell the agent *where* to look |
| **Knowledge Repo** | Theory, methodology, decisions, learning notes | Git-versioned | What the agent *knows* |
| **Engineering Repo** | Code, data pipelines, configs, tools | Git-versioned | What the agent *runs* |

> **The split:** Knowledge that makes sense without code → Knowledge Repo. Everything that needs execution → Engineering Repo.

---

### 🔄 Universal Workflow Pattern

Every agent, regardless of domain, runs the same **five-phase daily pattern**. What changes is what each phase *does*.

```
 PHASE        WHAT IT DOES                           YOUR CONFIG
───────  ─────────────────────────────────────  ──────────────────
 LEARN    Study new material, verify against     Your learning
          data, write structured notes            domain + cadence

 AUDIT    Scan knowledge base for stale pages,   Your freshness
          broken links, duplicates                thresholds

 PRODUCE  Synthesize information into a          Your audience
          deliverable — report, briefing,        + channel
          summary, code review

 MAINTAIN Ingest new data, run pipelines,         Your data
          update derived outputs                  sources

 SYNC     Push all repos + security scan          Your Git repos
```

> **The pattern is universal. The content is yours.**
>
> A **coding agent**: LEARN (new framework) → AUDIT (test coverage) → PRODUCE (PR review) → MAINTAIN (dependency update) → SYNC.
>
> A **research agent**: LEARN (new paper) → AUDIT (note freshness) → PRODUCE (briefing) → MAINTAIN (data pipeline) → SYNC.

---

### 📋 Task Priority Framework

Every agent faces conflicts. Four rules resolve them — domain-agnostic:

```
PRIORITY 1 — Core Mission
  ├── Time-critical outputs (deadline-driven deliverables)
  ├── Input-dependent tasks (runs when data arrives)
  └── Continuous improvement (learning, skill-building)

PRIORITY 2 — Amplification
  └── Creative work, writing, extended analysis

PRIORITY 3 — Infrastructure
  └── Tool maintenance, repo hygiene, system health
```

| # | Rule | Meaning |
|---|------|---------|
| 1 | **Time-Critical > Deep** | Deliverable due in 5 min beats brilliant insight |
| 2 | **Input-Dependent > Curiosity** | Pipeline runs when data lands; learning can wait |
| 3 | **Corrective > Additive** | Fix a bug before writing new code |
| 4 | **User-Triggered > Scheduled** | Human request pauses all automation |

---

### 📐 Methodology Evolution

The agent improves by **making mistakes and refusing to repeat them.**

```
1. Error occurs      →  Real output was wrong
2. Root cause found  →  "The framework was blind to this case"
3. Rule added        →  Specific check, not vague principle
4. Tested backward   →  Would this rule have caught past errors?
5. Version bump      →  Old version archived, never deleted
```

| Version | What Changed | Trigger |
|---------|-------------|---------|
| **v1** | Basic layered approach | Initial design |
| **v2** | Added type/domain classification | Errors in edge cases |
| **v3** | Common-factor contamination check | Misattributed macro move to domain signal |
| **v4** | Special-case rules + deeper model | Blind spot in existing framework |

> **Pattern, not prescription.** Your domain's versions will differ. The *process* matters: error → root cause → rule → test → bump.

---

### 📖 Navigation

| Page | What |
|------|------|
| **[`GETTING_STARTED.md`](GETTING_STARTED.md)** | **👈 Start here:** step-by-step replication guide |
| [`architecture.md`](architecture.md) | Three-layer memory model, repo split, data flow |
| [`workflow.md`](workflow.md) | Five-phase daily pattern, quality gates, sync |
| [`task-priority.md`](task-priority.md) | Priority framework, conflict resolution, failure handling |
| [`methodology/`](methodology/) | Error-driven evolution, knowledge taxonomy |
| [`decisions/`](decisions/) | Architecture Decision Records (ADR) |
| [`templates/`](templates/) | Page template, ADR template |
| [`index.md`](index.md) | Full directory |
| [`log.md`](log.md) | Changelog |

---

### 🧬 Design Philosophy

| # | Principle |
|---|-----------|
| 1 | **Every page answers a query** — no "just in case" content |
| 2 | **Memory is layered** — pointers → knowledge → code |
| 3 | **Decisions are ADRs** — context, options, rationale, revisit |
| 4 | **Write after verifying** — theory without data test stays draft |
| 5 | **The agent curates itself** — human reviews, not gatekeeps |
| 6 | **Archive, never delete** — preserve the evolution trail |

<br>

---

<span id="中文"></span>

## 中文

---

### 🎯 这是什么

**AgentHowTo** 是一套领域无关的参考架构——描述 AI Agent 如何管理长期记忆、持续学习、排定工作优先级、进化自己的方法论。跨会话、无需微调、只用 Markdown 和 Git。

这不是针对某个具体场景的教程。它是任何领域 Agent 都可以采用的**结构模式**。金融研究 Agent？接入市场数据。编程 Agent？接入代码审查。创意助手？接入内容迭代。

| 包含 | 不含 |
|---|---|
| ✅ 记忆架构（三层模型） | ❌ 特定领域的实现细节 |
| ✅ 工作流模式（学习→审计→产出→维护） | ❌ 锁死的时间表或工具 |
| ✅ 任务优先级框架（4 条决策规则） | ❌ "最佳提示词" |
| ✅ 方法论进化（错误驱动迭代） | ❌ 具体分析技巧 |
| ✅ 决策记录（ADR 格式） | ❌ 成品或产出 |

> **具体示例：** 文中穿插的示例框展示了一个金融研究 Agent 如何实例化这些模式。架构本身适用于任何领域。

---

### 🏗️ 架构

```
                    ┌─────────────────────────────────┐
                    │         AGENT 运行时             │
                    │   (会话上下文 + 工具)              │
                    └──────────────┬──────────────────┘
                                   │
          ┌────────────────────────┼────────────────────────┐
          │                        │                        │
    ┌─────▼──────┐          ┌─────▼──────┐          ┌─────▼──────┐
    │  MEMORY.md  │          │  知识仓库   │          │  工程仓库   │
    │  (指针层)    │          │  (大脑)     │          │  (双手)     │
    │             │          │            │          │            │
    │ "去哪找"     │          │ 理论       │          │ 代码       │
    │   ~2KB      │          │ 方法       │          │ 数据       │
    │             │          │ 决策       │          │ 配置       │
    └─────────────┘          └─────┬──────┘          └─────┬──────┘
          │                        │                        │
          └────────────────────────┼────────────────────────┘
                                   │
                          ┌────────▼────────┐
                          │  精选输出        │
                          │  (外部知识库)    │
                          │  打磨后的洞察    │
                          └──────────────────┘
```

**三层记忆模型：**

| 层级 | 存储内容 | 持久性 | 作用 |
|------|---------|--------|------|
| **MEMORY.md** | 指针、偏好、环境事实 | 每次会话注入 | 告诉 Agent 去哪找 |
| **知识仓库** | 理论、方法论、决策、学习笔记 | Git 版本控制 | Agent 知道什么 |
| **工程仓库** | 代码、数据管道、配置、工具 | Git 版本控制 | Agent 运行什么 |

> **分层原则：** 离开代码也能成立的知识 → 知识仓库。需要执行才能存在的东西 → 工程仓库。

---

### 🔄 通用工作流模式

任何 Agent，无论领域，都遵循同一个**五阶段日循环**。不同的是每个阶段做什么。

```
 阶段       做什么                              你的配置
──────  ──────────────────────────────────  ──────────────
 学习    学习新材料，用数据验证，写出结构化笔记   你的学习领域+节奏

 审计    扫描知识库：过期页面、失效链接、重复内容  你的新鲜度阈值

 产出    综合信息生成交付物：报告、简报、代码审查  你的受众+渠道

 维护    接入新数据、运行管道、更新衍生输出       你的数据源

 同步    推送所有仓库 + 安全扫描                  你的 Git 仓库
```

> **模式通用，内容自定。**
>
> **编程 Agent**：学习（新框架）→ 审计（测试覆盖率）→ 产出（代码审查）→ 维护（依赖更新）→ 同步。
>
> **研究 Agent**：学习（新论文）→ 审计（笔记新鲜度）→ 产出（简报）→ 维护（数据管道）→ 同步。

---

### 📋 任务优先级框架

每个 Agent 都会遇到冲突。四条规则解决——领域无关：

```
优先级 1 — 核心任务
  ├── 时间敏感产出（有截止时间的交付物）
  ├── 输入依赖任务（数据到了就跑）
  └── 持续改进（学习、技能建设）

优先级 2 — 扩展
  └── 创意工作、写作、深度分析

优先级 3 — 基础设施
  └── 工具维护、仓库卫生、系统健康检查
```

| # | 规则 | 含义 |
|---|------|------|
| 1 | **时间敏感 > 深度** | 5 分钟后到期的交付物，比精彩洞察优先 |
| 2 | **输入依赖 > 好奇心** | 数据到了先跑管道，学习可以等 |
| 3 | **修正 > 新增** | 先修 bug，再写新代码 |
| 4 | **用户触发 > 定时** | 人的请求暂停一切自动化 |

---

### 📐 方法论进化

Agent 不靠读更好的论文变聪明。靠**犯错误并拒绝重犯同样的错误**。

```
1. 错误发生      →  真实产出出了问题
2. 找到根因      →  "框架对这个情况是盲的"
3. 添加规则      →  具体的检查项，不是笼统的原则
4. 回测验证      →  这条规则能捕捉到过去的错误吗？
5. 版本升级      →  旧版本归档，永不删除
```

| 版本 | 改动 | 触发原因 |
|------|------|---------|
| **v1** | 基础分层方法 | 初始设计 |
| **v2** | 增加类型/领域分类 | 边界案例出错 |
| **v3** | 共因子污染检查 | 把宏观驱动错判成领域信号 |
| **v4** | 特殊规则 + 更深模型 | 现有框架的盲点 |

> **模式，不是药方。** 你的领域的"版本"看起来会不一样。重要的是过程：错误 → 根因 → 规则 → 回测 → 升级。

---

### 📖 导航

| 页面 | 内容 |
|------|------|
| **[`GETTING_STARTED.md`](GETTING_STARTED.md)** | **👈 从这里开始：** 一步步复刻指南 |
| [`architecture.md`](architecture.md) | 三层记忆模型、仓库分工、数据流 |
| [`workflow.md`](workflow.md) | 五阶段工作流模式、质量门禁、同步 |
| [`task-priority.md`](task-priority.md) | 优先级框架、冲突解决、失败处理 |
| [`methodology/`](methodology/) | 错误驱动进化、知识分类 |
| [`decisions/`](decisions/) | 架构决策记录 (ADR) |
| [`templates/`](templates/) | 页面模板、ADR 模板 |
| [`index.md`](index.md) | 完整目录 |
| [`log.md`](log.md) | 变更日志 |

---

### 🧬 设计哲学

| # | 原则 |
|---|------|
| 1 | **每页回答一个明确问题** — 不存"万一有用"的内容 |
| 2 | **记忆分层** — 指针 → 知识 → 代码 |
| 3 | **决策用 ADR 格式** — 背景、选项、理由、回顾 |
| 4 | **先验证再记录** — 没有数据验证的理论留在草稿 |
| 5 | **Agent 自我策展** — 人类审查，不做守门人 |
| 6 | **归档不删除** — 保留进化轨迹 |

<br>

---

<p align="center">
  <sub>Written by <a href="https://github.com/LibertyWayne">Vivian</a> · a reference architecture · adapt and remix freely</sub>
</p>
