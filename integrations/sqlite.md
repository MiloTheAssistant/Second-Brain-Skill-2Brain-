# Integration: SQLite ↔ 2Brain

**File:** `2brain.db` (gitignored — local index only)  
**Purpose:** Fast full-text search index over wiki/ for agent queries

---

## What SQLite Does

The wiki is flat markdown files — great for humans and AI, but slow for keyword search across large knowledge bases. SQLite provides a local index that enables fast queries without reading every file.

The SQLite database is a **derived index** — it can always be rebuilt from wiki/. It is never the source of truth.

---

## Schema

```sql
CREATE TABLE topics (
  id INTEGER PRIMARY KEY,
  title TEXT NOT NULL,
  path TEXT NOT NULL,            -- relative path from repo root, e.g. wiki/topic-name.md
  summary TEXT,                  -- first paragraph of the wiki article
  updated_at TEXT                -- ISO 8601 date
);

CREATE TABLE links (
  from_id INTEGER REFERENCES topics(id),
  to_id INTEGER REFERENCES topics(id)
);

CREATE VIRTUAL TABLE topics_fts USING fts5(
  title,
  summary,
  content='topics'
);
```

---

## Rebuilding the Index

Run this when wiki/ changes significantly or the index is stale:

```bash
# Via MCP SQLite tool or Claude Code:
# 1. Clear existing data
# 2. Walk wiki/ and insert one row per .md file
# 3. Extract first paragraph as summary
# 4. Parse [[cross-links]] and insert link rows
# 5. Rebuild FTS index
```

A rebuild script will be added to `scripts/rebuild-sqlite-index.md` when the wiki grows large enough to warrant it.

---

## MCP SQLite Server Config

```json
{
  "sqlite-2brain": {
    "command": "npx",
    "args": ["-y", "@modelcontextprotocol/server-sqlite",
             "/home/joscott/repos/Second-Brain-Skill-2Brain/2brain.db"]
  }
}
```

Claude Code and OpenClaw agents with MCP access can query the index directly:
```sql
SELECT path, summary FROM topics_fts WHERE topics_fts MATCH 'model routing' ORDER BY rank;
```

---

## .gitignore Entry

`2brain.db` must be gitignored — it is a local cache, not source:
```
2brain.db
```

---

## Contract

| Property | Value |
|----------|-------|
| File location | `<repo-root>/2brain.db` |
| Gitignored | Yes — never commit |
| Source of truth | `wiki/*.md` files |
| Rebuild trigger | After `compile-wiki` adds 10+ new articles |
| Access | MCP SQLite server or direct sqlite3 CLI |
