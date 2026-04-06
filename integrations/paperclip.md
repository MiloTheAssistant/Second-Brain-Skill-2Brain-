# Integration: PaperClip ↔ 2Brain

**Repo:** `github.com/MiloTheAssistant/PaperClip`  
**Direction:** PaperClip orchestrates 2Brain as a managed capability; routes queries and outputs

---

## How PaperClip Uses 2Brain

### Scheduled Maintenance
PaperClip triggers recurring 2Brain maintenance workflows:

| Schedule | Workflow |
|----------|----------|
| Weekly (Sunday 23:00) | `scripts/compile-wiki.md` — compile raw/ → wiki |
| Monthly (1st of month) | `scripts/health-check.md` — audit for contradictions/gaps |
| On demand | `scripts/ingest-source.md` — triggered by Composio MCP ingest events |

### Knowledge Query Routing
When any PaperClip-managed agent needs knowledge:
1. PaperClip routes the query to the 2Brain task
2. 2Brain runs `scripts/ask-question.md`
3. Answer + output file path returned to the requesting agent

### Output Routing
When agents complete tasks that produce knowledge:
1. PaperClip captures the output
2. Routes it to 2Brain via `scripts/ingest-source.md`
3. 2Brain integrates it into the knowledge base

---

## PaperClip Task Config

2Brain appears in PaperClip as a managed agent task with these capabilities:

```json
{
  "agent": "2Brain",
  "capabilities": ["ask", "ingest", "compile-wiki", "health-check"],
  "schedule": {
    "compile-wiki": "0 23 * * 0",
    "health-check": "0 9 1 * *"
  },
  "governance": {
    "requires_approval": ["compile-wiki", "health-check"],
    "auto_approve": ["ask", "ingest"]
  }
}
```

---

## Cost Tracking

PaperClip tracks 2Brain token usage per operation type:
- `compile-wiki` — largest operation; Sagan-level token budget
- `ask-question` — medium; scoped to wiki articles only
- `ingest-source` — small; single file processing
- `health-check` — medium; full wiki read

---

## Contract

| Property | Value |
|----------|-------|
| Trigger path | PaperClip task API → 2Brain skill endpoint |
| Output path | `outputs/YYYY-MM-DD-<slug>.md` (path returned to PaperClip) |
| Auth | PaperClip API key via `PAPERCLIP_OPENCLAW_TOKEN` |
| Governance | Approval gates for compile/health-check; auto for ask/ingest |
