# 2Brain — Command Center Shared Knowledge Base

A Karpathy-inspired flat-file second brain. Shared memory layer for OpenClaw, ClawCode, and PaperClip.

---

## What Is This

2Brain is a persistent, AI-maintained knowledge base built on plain markdown files. No databases, no plugins — just folders and prompts. Every interaction is designed to leave the knowledge base better than it found it.

Inspired by Andrej Karpathy's "super simple and flat" philosophy: the structure should be invisible so the knowledge can compound.

---

## Structure

```
raw/        ← source material (drop anything here — AI organises it)
wiki/       ← AI-maintained knowledge articles, cross-linked
outputs/    ← generated answers, reports, analyses (date-prefixed)
scripts/    ← prompt-as-script workflows (markdown files, not code)
integrations/ ← contracts for OpenClaw, ClawCode, PaperClip, Composio, SQLite
```

---

## Quick Start

**Add a source:**
Drop a file or paste content into `raw/`. Follow naming guidance in `raw/_INTAKE.md`.

**Compile the wiki:**
Run the prompt in `scripts/compile-wiki.md` — it reads `raw/`, creates/updates wiki articles, and maintains `wiki/INDEX.md`.

**Ask a question:**
Run `scripts/ask-question.md` — grounds answers in the wiki, saves output to `outputs/`, updates wiki with new insights.

**Health check:**
Run `scripts/health-check.md` monthly — flags contradictions, missing topics, unsourced claims.

---

## The Compounding Loop

```
raw/ ← (ingest-source, Composio MCP, agent outputs)
  ↓  compile-wiki
wiki/ ← (AI-maintained, cross-linked, SQLite indexed)
  ↓  ask-question
outputs/ ← (dated answers + analyses)
  ↓  feeds back
wiki/ ← (updated with new insights)
```

Every query improves the knowledge base. Every agent interaction adds to `raw/`. The base compounds over time.

---

## Integration

| System | How It Connects |
|--------|----------------|
| OpenClaw | Agents query wiki before acting; conversation outputs feed raw/ |
| ClawCode | Loads CLAUDE.md + wiki/codebase-patterns.md before coding |
| PaperClip | Schedules compile-wiki + health-check; routes knowledge queries |
| Composio MCP | Ingests from GitHub, Google Drive, Slack into raw/ |

See `integrations/` for full contracts.

---

## Attribution

Approach inspired by [Andrej Karpathy's second brain](https://github.com/karpathy) — flat files, no abstraction, just compounding knowledge.
