# Getting Started · 上手指南

> *Fork this repo, follow these steps, and your AI agent will have a persistent memory system in ~30 minutes.*

---

## Overview · 概述

This guide walks you through replicating the AgentHowTo cognitive architecture with your own AI agent. No specific agent framework required — works with Claude, GPT, Hermes, or any agent that can read and write Markdown files via Git.

**What you'll build:**

```
Your Agent
    │
    ├── MEMORY.md           ← pointer table (~2KB, injected each session)
    │
    ├── knowledge-repo/     ← your Knowledge Repo (brain)
    │   ├── learning/
    │   ├── decisions/
    │   ├── methodology/
    │   └── ...
    │
    └── engineering-repo/   ← your Engineering Repo (hands)
        ├── scripts/
        ├── config/
        └── ...
```

**Prerequisites:**
- An AI agent that can read/write files and run shell commands
- Git installed and configured
- A GitHub account (or any Git host)

**Time:** ~30 minutes for basic setup, ~2 hours with cron automation.

---

## Step 1: Fork & Clone · 复刻与克隆

```bash
# Fork this repo on GitHub, then:
git clone https://github.com/YOUR_USERNAME/AgentHowTo.git
cd AgentHowTo

# Create your own Knowledge Repo
mkdir -p ~/knowledge-repo/{learning,decisions,methodology,domain,archive}

# Create your own Engineering Repo (optional, if you have code/data)
mkdir -p ~/engineering-repo/{scripts,config,tools}
```

---

## Step 2: MEMORY.md — The Pointer Table · 指针表

Create `MEMORY.md` at a path your agent reads every session. Keep it under ~2 KB.

```markdown
# Agent Memory Pointers

## Knowledge Base
- Full knowledge → ~/knowledge-repo/ (Git: github.com/YOU/knowledge-repo)
- Architecture docs → ~/AgentHowTo/

## Environment
- OS: Ubuntu 22.04
- Python: 3.12
- Shell: bash

## Preferences
- User prefers concise responses
- Write in English by default

## Active Context
- Current project: [describe your main project]
- Last learning topic: [what you studied last]
```

**Rules:**
1. Only put *pointers* here — not full knowledge
2. Update when environment changes or user gives new preferences
3. If it grows past ~2 KB, move content to Knowledge Repo and leave a pointer

---

## Step 3: Knowledge Repo Structure · 知识仓库结构

Initialize your Knowledge Repo:

```bash
cd ~/knowledge-repo
git init
```

Create the directory structure:

```
knowledge-repo/
├── learning/           ← Theory + data verification notes
│   ├── domain-a/
│   ├── domain-b/
│   └── ...
├── methodology/        ← How your agent analyzes things
├── decisions/          ← Architecture Decision Records
├── domain/             ← Domain-specific analysis methods
├── archive/            ← Old content, never deleted
├── index.md            ← Live directory (update on every change)
├── log.md              ← Changelog
└── README.md           ← Repo overview
```

**Copy templates from AgentHowTo:**

```bash
cp ~/AgentHowTo/templates/page-template.md ~/knowledge-repo/templates/
cp ~/AgentHowTo/templates/adr-template.md ~/knowledge-repo/templates/
```

---

## Step 4: Configure Your Agent · 配置你的 Agent

### 4.1 Tell your agent to read before acting

Add this to your agent's system prompt or configuration:

```
Before any task, scan these files for relevant context:
1. ~/MEMORY.md — environment facts, preferences, pointers
2. ~/knowledge-repo/index.md — what knowledge is available

After learning something new, write to:
1. ~/knowledge-repo/learning/[topic].md — structured note
2. ~/knowledge-repo/index.md — update directory
3. ~/knowledge-repo/log.md — log the change
```

### 4.2 Set up the Knowledge Taxonomy

Give your agent these rules for deciding what to record:

```
When you learn something:

→ Theory tested against real data → learning/
→ How to analyze something better → methodology/
→ A technical choice made → decisions/
→ Transient (what happened today) → journal/ (private, not in Git)
→ Interesting but unverified → DISCARD
→ Already covered elsewhere → cross-reference, don't duplicate
```

### 4.3 Set up the Priority Stack

```
Task priority order:
1. Time-critical outputs (deadlines)
2. Input-dependent tasks (data just arrived)
3. Corrective work (fix bugs before new features)
4. User-triggered requests (pause automation)
5. Continuous learning (lowest priority, fill gaps)
```

---

## Step 5: Set Up the Five-Phase Workflow · 五阶段工作流

### Phase 1: LEARN

Create a cron job that triggers daily learning:

```bash
# Example: run every day at 2 AM
# Your agent should:
# 1. Pick next topic from learning queue
# 2. Study it (read docs, search, analyze)
# 3. Verify against real data
# 4. Write structured note to ~/knowledge-repo/learning/
# 5. Update index.md
```

### Phase 2: AUDIT

Create a cron job for knowledge base health:

```bash
# Example: run daily at 4 AM
# Your agent should check:
# - Pages untouched > 7 days → flag as stale
# - Cross-reference links → validate
# - index.md accuracy → compare with actual files
# - Frontmatter completeness → flag missing fields
```

### Phase 3: PRODUCE

Create a cron job for your daily deliverable:

```bash
# Example: run at 7 AM (before team standup)
# Your agent should:
# 1. Fetch data from your sources
# 2. Synthesize into a briefing/report/summary
# 3. Push to your team channel
```

### Phase 4: MAINTAIN

Create a cron job for data ingestion:

```bash
# Example: run after your data source updates
# Your agent should:
# 1. Fetch new data from APIs
# 2. Validate and clean
# 3. Store in your database
# 4. Recalculate derived outputs
```

### Phase 5: SYNC

Create a cron job for Git sync:

```bash
# Example: run nightly
# Your agent should:
# 1. cd ~/knowledge-repo && git add -A && git commit -m "daily sync" && git push
# 2. Run credential scan before push (no API keys in commits!)
```

---

## Step 6: Git Hygiene · 版本控制纪律

```bash
# Add this to your sync script (git_sync.sh):
#!/bin/bash
set -e

# Security scan before push
if grep -rPi '(api[_-]?key|token|secret|password|auth_token)\s*[=:]\s*[A-Za-z0-9_\-]{20,}' ~/knowledge-repo/; then
    echo "CREDENTIAL LEAK DETECTED. Aborting push."
    exit 1
fi

cd ~/knowledge-repo
git add -A
git commit -m "auto: daily sync $(date +%Y-%m-%d)" || true
git push
```

Make it executable: `chmod +x ~/git_sync.sh`

---

## Step 7: Customize for Your Domain · 适配你的领域

The architecture is domain-agnostic. Here's how to adapt it:

### If you're a Financial Research Agent:
- **LEARN:** Macroeconomics, asset pricing, commodity models
- **PRODUCE:** Morning market briefing
- **MAINTAIN:** Market data pipeline after close
- **Knowledge Repo:** learning/economics/, learning/finance/, learning/fixed_income/

### If you're a Coding Agent:
- **LEARN:** New frameworks, language features, architecture patterns
- **PRODUCE:** Code review summaries, PR analysis
- **MAINTAIN:** Dependency updates, test coverage reports
- **Knowledge Repo:** learning/languages/, learning/frameworks/, learning/architecture/

### If you're a Creative Agent:
- **LEARN:** Writing techniques, design patterns, narrative structures
- **PRODUCE:** Draft chapters, design mockups, content calendars
- **MAINTAIN:** Content pipeline, asset organization
- **Knowledge Repo:** learning/writing/, learning/design/, learning/narrative/

**The pattern is identical. Only the content changes.**

---

## Step 8: Verification Checklist · 验证清单

After setup, verify everything works:

- [ ] Agent reads `MEMORY.md` at session start
- [ ] Agent writes learning notes to `~/knowledge-repo/learning/`
- [ ] Agent updates `index.md` after writing
- [ ] Journal entries stay local (gitignored)
- [ ] Git sync runs without credential leaks
- [ ] Cron jobs trigger on schedule
- [ ] Knowledge Repo grows organically (not a dump)

---

## Troubleshooting · 常见问题

| Problem | Solution |
|---------|----------|
| Agent ignores MEMORY.md | Check the path in your agent's system prompt |
| Knowledge Repo gets bloated | Run AUDIT phase more frequently; be stricter about taxomony |
| Cron jobs overlap | Increase gaps between phases; check actual runtimes |
| Agent writes duplicates | Add cross-reference check before writing new pages |
| Git push fails | Run credential scan; check remote URL and permissions |

---

## Next Steps · 下一步

1. **Week 1:** Run all five phases manually. Observe what breaks.
2. **Week 2:** Automate with cron. Tighten gaps.
3. **Month 1:** Review Knowledge Repo. Archive stale pages. Tune taxonomy.
4. **Month 3:** First methodology version bump. What did your agent learn from its mistakes?

> **The architecture is a starting point. Your agent will make it its own.**
>
> **这个架构是起点。你的 Agent 会让它变成自己的。**
