# ClawCode → 2Brain Integration Contract

## Read Path
- Coding agent reads `wiki/` for technical context before implementing tasks
- `query.py` reads `data/brain.sqlite` for cost and memory stats

## Write Path
- Architecture decisions and debugging notes from coding sessions → `raw/` as source material
- Schema migrations live in `ClawCode/tools/sqlite/` and run against `data/brain.sqlite`

## Rules
- ClawCode never modifies `wiki/` directly — only adds to `raw/` for compilation
- Schema changes require a migration file in `ClawCode/tools/sqlite/`
- Test queries against brain.sqlite before deploying dashboard panels
