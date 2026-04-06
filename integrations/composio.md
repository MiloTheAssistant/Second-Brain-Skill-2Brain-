# Integration: Composio MCP ↔ 2Brain

**Service:** Composio MCP (`@composio-dev/mcp-server`)  
**Direction:** Composio ingests external content into 2Brain raw/

---

## What Composio Does for 2Brain

Composio MCP is the ingestion pipeline — it connects 2Brain to external knowledge sources and automatically drops content into `raw/`.

### Connected Sources

| Source | Content | Filename Format |
|--------|---------|----------------|
| GitHub | Repo summaries, PR descriptions, README changes | `raw/YYYY-MM-DD-github-<repo>-<slug>.md` |
| Google Drive | Documents, notes, meeting records | `raw/YYYY-MM-DD-gdrive-<doc-slug>.md` |
| Slack | Thread summaries, decisions, key messages | `raw/YYYY-MM-DD-slack-<channel>-<slug>.md` |
| Google Calendar | Meeting notes and action items | `raw/YYYY-MM-DD-gcal-<meeting-slug>.md` |

### Trigger Conditions

Composio writes to `raw/` when:
- A GitHub PR is merged in any of the four repos
- A Google Drive document is updated (watched documents only)
- A Slack thread is marked `#2brain` or contains a configured keyword
- A calendar meeting with a notes doc is completed

---

## Post-Ingest Workflow

After Composio writes to `raw/`:
1. PaperClip detects the new file (via webhook or poll)
2. PaperClip triggers `scripts/ingest-source.md` for immediate wiki integration
3. Or: file waits for the next scheduled `compile-wiki` run (Sunday night)

---

## Configuration

Composio MCP config lives in Claude Code's MCP settings (`~/.claude/settings.json`):
```json
{
  "composio": {
    "command": "npx",
    "args": ["-y", "@composio-dev/mcp-server"],
    "env": { "COMPOSIO_API_KEY": "${COMPOSIO_API_KEY}" }
  }
}
```

Composio tools available to Claude Code:
- `github` — repo operations, PR management
- `google-workspace` — Drive, Calendar, Gmail, Sheets
- `slack` — channel reading, thread summarisation
- `discord` — channel monitoring (Zuck's output feed)
- `vercel` — deployment monitoring

---

## Contract

| Property | Value |
|----------|-------|
| Write path | `raw/YYYY-MM-DD-<source>-<slug>.md` |
| Format | Markdown — source context + content |
| Auth | `COMPOSIO_API_KEY` in `.env` |
| On ingest | PaperClip triggers `ingest-source.md` or queues for `compile-wiki` |
