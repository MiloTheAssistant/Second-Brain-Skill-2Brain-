# raw/ — Intake Instructions

Drop source material here. The AI organises it — you don't need to.

---

## What Belongs Here

- Research notes, articles, bookmarks (paste as .md or .txt)
- PDFs, documents (exported as markdown or plain text)
- Agent conversation logs and task outputs
- Code architecture decisions and debugging notes
- Meeting notes, ideas, observations
- Web clippings, screenshots (as markdown with context)

---

## Naming Convention

No strict requirement — use something descriptive:

```
YYYY-MM-DD-topic-or-source.md
YYYY-MM-DD-openclaw-<agent>-<task>.md
YYYY-MM-DD-composio-github-<repo>.md
YYYY-MM-DD-slack-<channel>-<summary>.md
```

---

## Supported Formats

- `.md` — preferred
- `.txt` — accepted
- `.json` — accepted (will be summarised into wiki)

For PDFs and images: export/convert to text first, then drop here.

---

## What Happens Next

After adding files, run `scripts/compile-wiki.md` to:
- Create or update wiki articles from the new source
- Update `wiki/INDEX.md` with any new topics
- Cross-link related articles

Or run `scripts/ingest-source.md` for a single file ingestion with immediate wiki update.

---

## Agent Usage

OpenClaw agents write summaries here after completing tasks:
```
raw/YYYY-MM-DD-openclaw-<AgentName>-<task-slug>.md
```

Composio MCP writes ingested content here from GitHub, Google Drive, and Slack.

---

## Important

**Do not modify files already in `raw/` after they have been processed into wiki.**  
If a source is outdated, add a new file with the correction rather than editing the original.  
The wiki marks outdated information — the raw file is the permanent record.
