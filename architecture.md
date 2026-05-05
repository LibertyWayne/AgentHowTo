# Architecture · 架构

> *How an AI agent remembers across sessions — without a database, without fine-tuning, using only Markdown and Git.*

---

## The Core Problem · 核心问题

AI agents are **stateless by default.** Every session starts from zero.

| Constraint | Reality |
|-----------|---------|
| Context window | ~200K tokens, but compresses aggressively with long conversations |
| Conversation history | Persists only within session; lost on /new |
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
         │  "where to find X → AgentWiki → domain/..."
         │
    ┌────▼─────────────────────────────────────────────┐
    │              PERSISTENT STORAGE                    │
    │                                                    │
    │  ┌──────────────────┐    ┌──────────────────┐     │
    │  │   AgentWiki       │    │   AgentEngine     │     │
    │  │   (brain)         │    │   (hands)         │     │
    │  │                   │    │                   │     │
    │  │  methodology/     │    │  futures/         │     │
    │  │  learning/        │    │  scripts/         │     │
    │  │  decisions/       │    │  research/        │     │
    │  │  domain/          │    │  tools/           │     │
    │  │  archive/         │    │  config/          │     │
    │  │                   │    │                   │     │
    │  │  "what I know"    │    │  "what I run"     │     │
    │  └──────────────────┘    └──────────────────┘     │
    │                                                    │
    │  ┌──────────────────┐                             │
    │  │  SiYuan Note      │  ← polished, curated output │
    │  │  (external KB)    │                             │
    │  └──────────────────┘                             │
    └────────────────────────────────────────────────────┘
```

### Layer 1: MEMORY.md (The Index)

**Size:** ~2 KB · **Injected every session**

MEMORY.md is NOT a knowledge base. It's a **pointer table** — it tells the agent *where* to look, not *what* to know.

```
✅ Good MEMORY entry:
   "品种代码全大写(如CU2605), 主力换月=v6成交量最大+115%粘性"

❌ Bad MEMORY entry:
   "The dominant contract algorithm uses volume maximum with 115% stickiness
    and avoids retired_ym because..." [3 paragraphs of detail → belongs in Wiki]
```

**Rules:**
- Only facts that prevent the user from having to repeat themselves
- Pointers to wiki pages, not the content itself
- ~2 KB cap — forces curation

### Layer 2: AgentWiki (The Brain)

**Size:** ~60 pages · **Git-versioned**

Contains everything that *makes sense without code*:

| Directory | What | Example |
|-----------|------|---------|
| `learning/` | Theory with data verification | Macroeconomics → how M2 transmits to commodity prices |
| `methodology/` | How the agent analyzes things | Commodity framework v1→v4 evolution |
| `decisions/` | Architecture Decision Records | Why DuckDB over PostgreSQL |
| `domain/` | Domain-specific analysis methods | Commodity analysis discipline: 6 principles |
| `archive/` | Superseded but preserved | Old analyses, retired frameworks |

### Layer 3: AgentEngine (The Hands)

**Size:** ~30 files · **Git-versioned**

Everything that *needs a database or script to exist*:

| Directory | What | Example |
|-----------|------|---------|
| `futures/` | Data pipeline + docs | Daily OHLCV from Tushare → DuckDB |
| `scripts/` | Collection automation | Broker position rankings, warehouse receipts |
| `research/` | Quant models | Factor construction, backtest frameworks |
| `tools/` | Tool references | DuckDB quirks, Tushare API patterns |
| `config/` | Environment | Cron schedules, dependencies |

---

## Why Two Repos? · 为什么两个仓库？

| Question | AgentWiki | AgentEngine |
|----------|-----------|-------------|
| What is it? | Knowledge | Infrastructure |
| Can it stand alone? | Yes — readable by any human | No — needs credentials, databases |
| Who is it for? | The agent + curious humans | The agent + maintainers |
| Sensitive content? | No (journal is gitignored) | No (credentials never in Git) |
| Public? | No (but structurally publishable) | No |

> **The split is functional, not cosmetic.** When the agent learns something new about yield curve dynamics, it writes to AgentWiki. When it fixes a bug in the data pipeline, it commits to AgentEngine. The boundary is: *would this still be useful if the database disappeared?*

---

## Data Flow · 数据流

```
External APIs                    Internal Processing                Storage
─────────────                    ────────────────────               ───────

Tushare ──────┐
              │
EastMoney ────┤                  ┌───────────────┐
              ├──→ Collect ──→   │  Data Clean   │──→ DuckDB INSERT
RSSHub ───────┤    scripts       │  + Validate   │       │
              │                  └───────────────┘       │
SunSirs ──────┘                                          │
                                                         │
CLS / WSCN ──→ Macro news ──→ fundamental_data table ←───┘
                                                         │
                                                         ▼
                                              ┌──────────────────┐
                                              │  Dominant Map    │
                                              │  Recalculation   │
                                              │  (v6: vol+115%)  │
                                              └──────────────────┘
                                                         │
                                                         ▼
                                              ┌──────────────────┐
                                              │  Continuous      │
                                              │  Series Views    │
                                              └──────────────────┘
```

The pipeline runs **Mon–Fri at 17:00 CST** via cron. Each step logs to a Feishu notification on completion.

---

## Self-Healing Properties · 自愈机制

The architecture has built-in resilience:

| Problem | How It's Handled |
|---------|-----------------|
| API rate limits | Exponential backoff in collection scripts |
| Stale wiki pages | Memory audit cron job detects unmaintained content |
| Credential leak | Git pre-push hook scans for API keys/tokens |
| Dominant contract error | Algorithm logs inconsistency rate; flags >2% |
| Context overflow | MEMORY.md enforces 10KB cap; wiki pages are short |

---

> **Next:** [Workflow · 工作流](workflow.md) — how the daily learning loop works
