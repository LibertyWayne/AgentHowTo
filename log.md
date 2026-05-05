# Changelog · 变更日志

> All significant changes to AgentHowTo documentation.

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

## Format

```
## [YYYY-MM-DD] type | Summary
- Bullet list of changes
- Links to affected pages
```
