---
name: 2brain
description: Second Brain knowledge base — query, ingest, compile, and health-check a persistent markdown wiki. Use when asked to look something up in the second brain, add knowledge, compile the wiki from raw sources, or run a health check on the knowledge base. Triggers on "second brain", "2brain", "knowledge base", "look up in wiki", "add to brain", "compile wiki", "ingest source".
metadata:
  {
    "openclaw":
      {
        "emoji": "🧠",
        "requires": { "files": ["Second-Brain-Skill-2Brain"] },
      },
  }
---

# 2Brain — Second Brain Knowledge Base

A Karpathy-inspired flat-file knowledge base. Persistent shared memory for OpenClaw, ClawCode, and PaperClip.

## When to Use

- User asks to **look up** information → use `ask` action
- User wants to **add knowledge** from a URL, file, or text → use `ingest` action
- User wants to **compile** or refresh the wiki from raw sources → use `compile` action
- User wants to **health-check** the knowledge base → use `health-check` action

## Actions

### `ask` — Query the knowledge base

1. Read `wiki/INDEX.md` to find relevant topic pages
2. Read those pages and synthesize an answer
3. Cite specific wiki pages: `(see: [[page-name]])`
4. If the answer is valuable, offer to save it as a new wiki page
5. If the answer is not in the wiki, say so — do not fabricate

### `ingest` — Add source material

1. Take the source content (text, URL content, file) and save it to `raw/YYYY-MM-DD-<slug>.md`
2. Run the compile workflow to update wiki articles from the new source
3. See [scripts/ingest-source.md](scripts/ingest-source.md) for the full prompt

### `compile` — Compile wiki from raw sources

1. Read all files in `raw/`
2. Create or update `wiki/<topic>.md` for each major topic
3. Cross-link related topics using `[[topic-name]]`
4. Update `wiki/INDEX.md`
5. See [scripts/compile-wiki.md](scripts/compile-wiki.md) for the full prompt

### `health-check` — Audit the knowledge base

1. Check for contradictions between wiki articles
2. Find topics mentioned but never given their own file
3. Flag unsourced claims
4. Suggest new articles to fill gaps
5. See [scripts/health-check.md](scripts/health-check.md) for the full prompt

## Key Paths

| Path | Purpose |
|------|---------|
| `raw/` | Source material (read-only for AI — never modify) |
| `wiki/` | AI-maintained knowledge articles |
| `wiki/INDEX.md` | Master index of all topics |
| `outputs/` | Generated answers and reports |
| `scripts/` | Prompt-as-script workflow templates |
| `integrations/` | Integration contracts for OpenClaw, ClawCode, PaperClip |

## Rules

- **Never modify files in `raw/`** — only add new files
- **Never delete wiki content** — mark outdated info with `[OUTDATED as of YYYY-MM-DD]`
- **Every wiki page** starts with a one-paragraph summary
- **Cross-link** related topics using `[[topic-name]]`
- **Source citations** — reference `raw/` files for factual claims
- **Outputs** are date-prefixed: `YYYY-MM-DD-<slug>.md`

## Full Schema

See [CLAUDE.md](CLAUDE.md) for the complete behavior rules governing all AI interactions with this knowledge base.

## OpenClaw Integration

See [integrations/openclaw.md](integrations/openclaw.md) for bidirectional contract details. Key points:

- **Read path:** `wiki/INDEX.md` and `wiki/<topic>.md`
- **Write path:** `raw/YYYY-MM-DD-openclaw-<agent>-<slug>.md`
- **Frequency:** Read at workflow start; write after significant task completion