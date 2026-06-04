# Hermes Agent 运作机制

> 最后更新：2026-06-04
> 本文档描述 Wayne 的 Hermes Agent 系统的完整运作机制。

---

## 一、系统架构

```
┌─────────────────────────────────────────────┐
│              硅谷云服务器 (Linux)              │
│                                             │
│  ┌─────────┐  ┌──────────┐  ┌────────────┐ │
│  │ Hermes   │  │ PostgreSQL│  │  Redis      │ │
│  │ Agent    │──│ +DuckDB  │  │  (缓存)     │ │
│  │ (Gateway)│  │ futures  │  │             │ │
│  └────┬─────┘  └──────────┘  └────────────┘ │
│       │                                      │
│  ┌────┴─────┐  ┌──────────┐  ┌────────────┐ │
│  │  飞书     │  │  Tushare  │  │   AKShare  │ │
│  │  Gateway  │  │  数据代理  │  │   数据源    │ │
│  └────┬─────┘  └──────────┘  └────────────┘ │
└───────┼──────────────────────────────────────┘
        │ 飞书 API
        ▼
┌───────────────┐
│    Wayne 的    │
│    飞书客户端   │
└───────────────┘
```

---

## 二、长时记忆系统

### 2.1 三层记忆架构

| 层级 | 存储位置 | 内容 | 生命周期 |
|------|---------|------|---------|
| **Memory** | `~/.hermes/memories/` | 用户偏好、环境事实、经验教训 | 跨 session 永久 |
| **User Profile** | `~/.hermes/profiles/default/user.md` | Wayne 的身份、偏好、工作方式 | 跨 session 永久 |
| **Session** | `~/.hermes/sessions/` | 单次对话的完整上下文 | 14天清理 |

### 2.2 Memory 写入规则

只存**会反复用到的事实**，不存任务进度和临时状态：

- ✅ 用户偏好（"Wayne偏好简洁回答"）
- ✅ 环境事实（"数据库在 /data/futures/futures_new.duckdb"）
- ✅ 经验教训（"推送前必须脱敏"）
- ❌ 任务状态（"已完成XX分析"）
- ❌ PR号、commit SHA、文件计数

### 2.3 会话检索

`session_search` 工具可全文检索历史会话。用于回答"我们之前讨论过XX"这类问题。

---

## 三、Skills 系统

### 3.1 什么是 Skill

Skill 是可复用的操作流程文档。Agent 在相关任务时自动加载，按既定步骤执行，避免每次都从头摸索。

位置：`~/.hermes/skills/`

### 3.2 核心 Skills

| Skill | 触发条件 | 用途 |
|-------|---------|------|
| `commodity-analysis-methodology` | 期货品种分析 | 三层分析框架（宏观→产业→技术面） |
| `commodity-analysis-discipline` | 期货品种分析 | 18条纪律规则 + 9个探针测试 |
| `variety-research-framework` | 品种研究 | 完整研究流程（定位→数据→分析→决策树） |
| `futures-database-handbook` | 数据库操作 | DuckDB 操作手册 |
| `hermes-agent` | Hermes 配置/运维 | CLI 命令、Gateway、Profiles |
| `daily-audit-loop` | 每日审计 | Cron 健康检查 + 问题闭环追踪 |
| `financial-morning-briefing` | 每日7:30 | 财经早报生成 |

### 3.3 Skills 生命周期

1. 任务成功后，复杂流程被保存为 Skill
2. 使用中发现不足，即时修补（patch）
3. 过时的 Skill 可归档或合并

---

## 四、Profiles 系统

### 4.1 多身份隔离

```bash
# 默认 Profile（当前使用）
~/.hermes/config.yaml
~/.hermes/memories/
~/.hermes/skills/

# 其他 Profile（独立配置、独立记忆、独立 Skills）
hermes profile create agent2
~/.hermes/profiles/agent2/config.yaml
~/.hermes/profiles/agent2/memories/
~/.hermes/profiles/agent2/skills/
```

### 4.2 当前使用

单一 Profile：`default`。通过飞书 Gateway 与 Wayne 交互。

---

## 五、Cron 定时任务

### 5.1 核心任务

| 任务 | 频率 | 功能 |
|------|------|------|
| 期货数据更新 | 17:00 周一至五 | Tushare行情+持仓+仓单 |
| 期货新闻采集 | 17:10 周一至五 | 宏观新闻+品种新闻+生意社 |
| 股票数据增量 | 17:30 周一至五 | A股因子数据更新 |
| 财经早报 | 7:30 每天 | 多源整合财经+科技新闻推送 |
| 每日审计 | 3:00 每天 | Cron健康检查+问题闭环 |
| 每日深度学习 | 1:00 每天 | 自主学习研究 |
| 全系统备份 | 4:30 每3天 | 数据库备份到COS |
| Git同步 | 22:00 每天 | AgentWiki + AgentEngine 推送到 GitHub |

### 5.2 任务特点

- Cron 任务在独立 session 运行，无当前对话上下文
- 可指定 `model`、`skills`、`enabled_toolsets` 优化 token
- 支持 `no_agent` 模式（纯脚本执行，0 token）
- 支持 `context_from` 串联（一个任务的输出注入另一个）

---

## 六、数据管道

### 6.1 数据源

| 源 | 用途 |
|----|------|
| Tushare Pro（via 代理） | 期货/股票日K线、品种信息 |
| AKShare | 仓单、现货基差 |
| 东方财富 / 财联社 | 宏观+品种新闻 |
| 生意社 | 品种产业数据 |
| FRED / MarketAux | 海外宏观+情绪数据 |

### 6.2 存储

| 数据库 | 路径 | 内容 |
|--------|------|------|
| DuckDB | `/data/futures/futures_new.duckdb` (112MB) | 期货K线、持仓、仓单、基本面 |
| PostgreSQL | 本地 | 品种元数据、交易时段模板 |
| Redis | 本地 | 技术指标缓存、纸面交易状态 |

### 6.3 关键数据

- 期货日K线：1,675,417 行，1995年至今
- 品种覆盖：98个（含6大交易所）
- 更新频率：交易日17:00

---

## 七、飞书 Gateway

### 7.1 连接模式

WebSocket 模式（推荐）：Agent 主动出站连接飞书，无需公网 IP。

### 7.2 行为规则

| 场景 | 行为 |
|------|------|
| 私聊 | 回复每条消息 |
| 群聊 | 仅在被 @时回复 |
| 多用户群聊 | 默认按用户隔离 session |

### 7.3 安全

- 用户白名单（`FEISHU_ALLOWED_USERS`）
- 危险命令审批卡片
- 群聊策略控制（allowlist/blacklist/open/admin_only）

---

## 八、知识库 (Wiki)

### 8.1 结构

```
~/wiki/learning/
├── commodities/      # 商品期货（黑色/能化/有色/农产品/贵金属）
├── economics/        # 经济学（宏观/微观/货币）
│   ├── macro/        # 财政、汇率、经济周期
│   ├── micro/        # 成本理论、弹性、市场结构
│   └── monetary/     # 利率传导、信用周期
├── finance/          # 金融理论（期权、行为金融、微观结构）
├── fixed_income/     # 固收（债券定价、ABS、可转债）
├── equities/         # 股票（因子、估值、行业轮动）
├── asset_allocation/ # 大类资产配置
├── quant/            # 量化方法
├── methodology/      # 方法论（回测陷阱、资产本体论）
├── investment_philosophy/  # 投资哲学
├── journal/          # 学习日志（每日记录）
└── references/       # 论文参考
```

### 8.2 核心理念

- **费曼学习法**：每个深度页面都是"教自己"的产物
- **诚实标注缺口**：明确哪些领域还没覆盖
- **跨域连线**：`knowledge_graph.md` 记录概念间的咬合关系
- **学习看板**：`LEARNING_BOARD.md` 追踪 Backlog→Ready→InProgress→Review→Done

### 8.3 同步

GitHub 私有仓库 `LibertyWayne/AgentWiki`，每日 22:00 自动推送。

---

## 九、Git 仓库

| 仓库 | 可见性 | 内容 |
|------|--------|------|
| `AgentWiki` | 私有 | 知识库、方法论、学习笔记（大脑） |
| `AgentEngine` | 私有 | 数据管道、数据库、脚本（双手） |
| `AgentHowTo` | **公开** | Agent 架构参考文档（脱敏） |

---

## 十、安全纪律

1. **API Key 永不出现在输出、代码、Git 历史中**
2. 配置文件（`.env`, `config.yaml`）不提交到 Git
3. 推送前自动脱敏检查
4. `hermes config set` 操作不暴露凭据
5. 飞书 Gateway 白名单限制访问用户
