# Changelog · 变更日志

> All significant changes to AgentHowTo documentation.

---

## [2026-05-05] feat | Added step-by-step replication guide

- `GETTING_STARTED.md` — 8-step tutorial: fork → MEMORY.md → Knowledge Repo → configure agent → cron workflow → Git hygiene → domain customization → verification checklist
- Added to README navigation (EN + CN) and index.md

---

## [2026-05-05] refactor | Genericized for public reference architecture

**Changes across all pages:**
- Removed all references to private repos (AgentWiki, AgentEngine) → replaced with generic "Knowledge Repo" / "Engineering Repo"
- SiYuan Note → "Curated Output" / "external knowledge base"
- Hardcoded cron times (01:00, 03:00, 07:30, 17:00, 22:00) → configurable time blocks (Early AM, Morning, Pre-Market, Post-Market, Night)
- Removed "Companion Repos" section from README and index
- Added "adapt to your own setup" notes throughout
- Workflow examples now explicitly marked as "example rotation" / "example structure"
- Data flow diagram genericized (Tushare → "Market API", Feishu → "Team Channel")

**Positioning:** This is now a conceptual reference architecture — adapt the pattern to your own agent, tools, and workflow.

---

## [2026-05-05] init | Repository created

**New pages:**
- `README.md` — Bilingual project overview with architecture diagram, workflow summary, philosophy
- `architecture.md` — Three-layer memory model, repo split rationale, data flow, self-healing
- `workflow.md` — Daily cron schedule, deep learning loop, quality gates, git hygiene
- `task-priority.md` — Priority stack, conflict resolution matrix, failure handling
- `methodology/framework-evolution.md` — Analytical framework v1 → v4.1 error-driven iteration
- `methodology/knowledge-taxonomy.md` — Storage decision matrix, archive/forget rules
- `decisions/sample-adr.md` — Real ADR: dominant contract v6 algorithm
- `templates/page-template.md` — Wiki page template with YAML frontmatter
- `templates/adr-template.md` — Architecture Decision Record template
- `index.md` — Full directory/navigation
- `log.md` — This file

**Design decisions:**
- English primary with Chinese subtitles (bilingual blockquotes where appropriate)
- README as Hero page — shows the full system at a glance
- Every page links to the next (narrative flow)
- Fancy but readable: badges, ASCII diagrams, tables, no walls of text

---

## [2026-06-06] docs | OPERATING_MECHANISMS v2 — 新增审计、交叉学习、Skill禁用等机制

**OPERATING_MECHANISMS.md 重大更新：**

- **新增 §三 审计机制**：三层审计（Cron健康检查 + 问题闭环 + 记忆审计），审计输出格式，审计纪律
- **新增 §2.4 Skill禁用纪律**：被Cron引用的Skill不可禁用，否则静默失败无报错
- **新增 §5.3 交叉学习纪律**：框架对接必须在同一时空尺度，跨频率硬接 = 废话
- **扩展 §一 记忆规则**：增加反例——不该存的内容（任务进度、PR编号、commit SHA等）
- **扩展 §八 Git仓库**：增加推送前脱敏安全检查
- **扩展 §十 通用纪律**：从7条扩展至10条
- **整体重组**：审计独立成章，节号重新编排（原§三→§四，以此类推）

---

## [2026-06-06] docs | 新增 BOOTSTRAP.md — 从零搭建完整工作体系

**BOOTSTRAP.md — 一份给 AI Agent 照做就能从零搭建完整体系的 checklist：**

- **Phase 0:** 搭建基础（三个仓库、Agent 能力确认）
- **Phase 1:** 记忆系统（三层记忆、存什么不存什么、自我检查）
- **Phase 2:** 知识框架（目录结构、费曼法、交叉学习、WIP=1）
- **Phase 3:** Skills（创建时机、标准结构、生命周期、禁用纪律）
- **Phase 4:** Cron 自动化（第一个任务、任务矩阵、串联、省钱技巧）
- **Phase 5:** 审计系统（三层审计、输出格式、闭环纪律）
- **Phase 6:** Git & 持续改进（脱敏、commit 规范、循环、版本管理）
- **完整验证清单** + 常见陷阱表

**设计理念：** 每 Phase 有明确的目标、具体步骤、自我检查清单。Agent 不需要理解原理——照做就行。

**其他变更：**
- index.md: BOOTSTRAP.md 置顶为核心入口
- README: 新增 BOOTSTRAP.md 导航链接

---

## Format

```
## [YYYY-MM-DD] type | Summary
- Bullet list of changes
- Links to affected pages
```
