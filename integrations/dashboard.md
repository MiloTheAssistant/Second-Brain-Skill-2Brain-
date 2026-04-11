# Dashboard → 2Brain Integration Contract

## Read Path (read-only)
The Command Center Dashboard reads from `data/brain.sqlite`:

| Table | Dashboard Panel | Query Pattern |
|---|---|---|
| `cost_tracker` | Cost breakdown by agent, provider, model | GROUP BY agent/provider, SUM tokens/cost |
| `memory_ops_log` | Memory transparency — what agents read/wrote | Recent ops, filtered by agent |
| `wiki/` directory | 2Brain stats — article count, last modified | Filesystem scan |
| `briefings/archive/` | DFB history — past briefings | Read JSON files by date |

## Write Path
Dashboard does NOT write to 2Brain. It is strictly read-only.

## Connection
Dashboard connects to brain.sqlite via `ClawCode/tools/sqlite/query.py` or direct SQLite reads from the Next.js API routes.

## Rules
- Never write to brain.sqlite from the dashboard
- Cache expensive queries (article count, total cost) — refresh every 5 minutes max
- Display timestamps in America/Chicago timezone for John
