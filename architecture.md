# Architecture · 架构

> *How an AI agent remembers across sessions — without a database, without fine-tuning, using only Markdown and Git.*

---

## The Core Problem · 核心问题

AI agents are **stateless by default.** Every session starts from zero.

| Constraint | Reality |
|-----------|---------|
| Context window | Large but compresses aggressively with long conversations |
| Conversation history | Persists only within session; lost on reset |
| Fine-tuning | Slow, expensive, can't capture *this week's discovery* |
| Vector databases | Retrieval-augmented, but noisy — retrieves fragments, not structured knowledge |

**The solution is not more technology. It's better structure.**

---

## Three-Layer Memory Model · 三层记忆模型

```
┌─────────────────────────────────────────────────────────┐
│                    SESSION CONTEXT                       │
│  (what the agent can "see" right now)                    │
│                                                          │
│  ┌──────────┐     ┌──────────┐     ┌──────────┐         │
│  │ MEMORY.md │     │  Skill   │     │  User    │         │
│  │ (pointers)│     │  Docs    │     │  Message │         │
│  └─────┬─────┘     └──────────┘     └──────────┘         │
│        │                                                  │
└────────┼──────────────────────────────────────────────────┘
         │  "where to find X → Knowledge Repo → domain/..."
         │
    ┌────▼─────────────────────────────────────────────┐
    │              PERSISTENT STORAGE                    │
    │                                                    │
    │  ┌──────────────────┐    ┌──────────────────┐     │
    │  │  KNOWLEDGE REPO   │    │ ENGINEERING REPO  │     │
    │  │  (brain)          │    │ (hands)           │     │
    │  │                   │    │                   │     │
    │  │  methodology/     │    │  pipeline/        │     │
    │  │  learning/        │    │  scripts/         │     │
    │  │  decisions/       │    │  research/        │     │
    │  │  domain/          │    │  tools/           │     │
    │  │  archive/         │    │  config/          │     │
    │  │                   │    │                   │     │
    │  │  "what I know"    │    │  "what I run"     │     │
    │  └──────────────────┘    └──────────────────┘     │
    │                                                    │
    │  ┌──────────────────┐                             │
    │  │  CURATED OUTPUT   │  ← external KB              │
    │  │  (polished notes) │    polished, human-readable │
    │  └──────────────────┘                             │
    └────────────────────────────────────────────────────┘
```

### Layer 1: MEMORY.md (The Index)

**Size:** ~2 KB · **Injected every session**

MEMORY.md is NOT a knowledge base. It's a **pointer table** — it tells the agent *where* to look, not *what* to know.

```
✅ Good MEMORY entry:
   "Dominant contract selection uses volume maximum + stickiness.
    Full docs → Knowledge Repo → decisions/002-dominant-algorithm"

❌ Bad MEMORY entry:
   "The dominant contract algorithm uses volume maximum with 115% stickiness
    and avoids retired_ym because..." [details belong in Knowledge Repo]
```

**Rules:**
- Only facts that prevent the user from repeating themselves
- Pointers to Knowledge Repo pages, not the content itself
- ~2 KB cap — forces curation, not dumping

### Layer 2: Knowledge Repo (The Brain)

**Size:** Configurable · **Git-versioned**

Contains everything that *makes sense without code*:

| Directory | What | Example |
|-----------|------|---------|
| `learning/` | Theory with data verification | Macroeconomics → how monetary policy transmits to markets |
| `methodology/` | How the agent analyzes things | Framework v1→v4 evolution |
| `decisions/` | Architecture Decision Records | Why this database over that one |
| `domain/` | Domain-specific analysis methods | Analysis discipline and principles |
| `archive/` | Superseded but preserved | Old analyses, retired frameworks |

### Layer 3: Engineering Repo (The Hands)

**Size:** Configurable · **Git-versioned**

Everything that *needs a database or script to exist*:

| Directory | What | Example |
|-----------|------|---------|
| `pipeline/` | Data pipeline + docs | Daily market data → storage → derived series |
| `scripts/` | Collection automation | Position rankings, warehouse receipts |
| `research/` | Quant models | Factor construction, backtest frameworks |
| `tools/` | Tool references | Database quirks, API patterns |
| `config/` | Environment | Schedules, dependencies |

### Bonus: Curated Output

A separate, polished knowledge base (e.g., Notion, Obsidian, a note-taking app of your choice) for **human-readable** output. The agent writes raw notes to the Knowledge Repo during learning, then curates the best insights into clean, structured output for humans to read.

---

## Why Two Repos? · 为什么两个仓库？

| Question | Knowledge Repo | Engineering Repo |
|----------|---------------|-----------------|
| What is it? | Knowledge | Infrastructure |
| Can it stand alone? | Yes — readable by any human | No — needs credentials, databases |
| Who is it for? | The agent + curious humans | The agent + maintainers |
| Sensitive content? | No | No (credentials never in Git) |

> **The split is functional, not cosmetic.** When the agent learns something new about yield curve dynamics, it writes to the Knowledge Repo. When it fixes a bug in the data pipeline, it commits to the Engineering Repo. The boundary is: *would this still be useful if the database disappeared?*

---

## Data Flow · 数据流

```
External APIs                    Internal Processing                Storage
─────────────                    ────────────────────               ───────

Market API ──────┐
                 │
News API ────────┤                  ┌───────────────┐
                 ├──→ Collect ──→   │  Data Clean   │──→ Database INSERT
Sector API ──────┤    scripts       │  + Validate   │       │
                 │                  └───────────────┘       │
Spot Price ──────┘                                          │
                                                            │
Macro News ──────→ News feed ──→ News table ←──────────────┘
                                                            │
                                                            ▼
                                                 ┌──────────────────┐
                                                 │  Derived Series  │
                                                 │  Recalculation   │
                                                 │  (algorithm vN)  │
                                                 └──────────────────┘
                                                            │
                                                            ▼
                                                 ┌──────────────────┐
                                                 │  Continuous      │
                                                 │  Series Views    │
                                                 └──────────────────┘
```

The pipeline runs on a cron schedule after your market closes. Each step logs on completion.

---

## Self-Healing Properties · 自愈机制

The architecture has built-in resilience:

| Problem | How It's Handled |
|---------|-----------------|
| API rate limits | Exponential backoff in collection scripts |
| Stale wiki pages | Memory audit job detects unmaintained content |
| Credential leak | Git pre-push hook scans for API keys/tokens |
| Algorithm error | Logs inconsistency rate; flags anomalies |
| Context overflow | MEMORY.md enforces size cap; wiki pages are short |

---

> **Next:** [Workflow · 工作流](workflow.md) — how the daily learning loop works
