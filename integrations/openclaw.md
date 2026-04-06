# Integration: OpenClaw ↔ 2Brain

**Repo:** `github.com/MiloTheAssistant/OpenClawMaster`  
**Direction:** Bidirectional — agents read from wiki, write summaries to raw/

---

## How OpenClaw Uses 2Brain

### Reading (before acting)
OpenClaw agents load relevant wiki context before executing tasks:

1. **CORTANA** — loads `wiki/INDEX.md` at the start of every workflow for project state context
2. **NEO** — reads `wiki/codebase-patterns.md` before architecture proposals
3. **SAGAN** — checks wiki for existing synthesis on research topics before running Perplexity queries
4. **CLAWCODE** — reads `wiki/codebase-patterns.md` and any repo-specific wiki before coding

### Writing (after completing tasks)
Agents write summaries back to `raw/` as source material:

```
raw/YYYY-MM-DD-openclaw-<AgentName>-<task-slug>.md
```

Recommended agents to write back:
- SAGAN: research synthesis summaries
- NEO: architecture decision records
- ELON: executive packets (sanitised)
- CORNELIUS: execution plan records

---

## Skill Integration

2Brain is available as an OpenClaw skill. Agents invoke it via:
```
skill: 2brain
action: ask | ingest | compile | health-check
query: <question or source content>
```

---

## Compounding Loop

```
OpenClaw task completes
  → Agent writes summary to raw/
  → ingest-source.md processes it into wiki
  → Next agent reads richer wiki context
  → Better task execution
```

---

## Contract

| Property | Value |
|----------|-------|
| Read path | `wiki/INDEX.md`, `wiki/<topic>.md` |
| Write path | `raw/YYYY-MM-DD-openclaw-<agent>-<slug>.md` |
| Format | Markdown — plain summary, facts, decisions |
| Frequency | Read: every workflow start. Write: after significant task completion. |
