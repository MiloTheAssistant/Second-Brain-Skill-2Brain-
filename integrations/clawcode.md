# Integration: ClawCode ↔ 2Brain

**Repo:** `github.com/MiloTheAssistant/ClawCode`  
**Direction:** Bidirectional — ClawCode reads patterns before coding, writes decisions after

---

## How ClawCode Uses 2Brain

### Session Start (always)
At the start of every ClawCode session:
1. Load `CLAUDE.md` from this repo (2Brain schema)
2. Read `wiki/INDEX.md` for relevant topics
3. Read `wiki/codebase-patterns.md` if it exists
4. Read any repo-specific wiki article if it exists (e.g., `wiki/openclawmaster-patterns.md`)

This ensures CLAWCODE implements code consistent with established patterns across all four repos.

### Before Writing Code
For every implementation task:
- Check wiki for existing patterns in the target language/framework
- Check for previous architecture decisions that apply
- If no wiki coverage exists: note the gap and proceed with best judgment

### After Implementation
Write an architecture decision record (ADR) to `raw/`:

```
raw/YYYY-MM-DD-clawcode-adr-<slug>.md
```

Include:
- What was implemented
- Why this approach (vs alternatives)
- Key patterns established
- What future code should follow

---

## Codebase Patterns Wiki Article

`wiki/codebase-patterns.md` is the canonical cross-repo patterns article. ClawCode should:
- Read it before every session
- Contribute new patterns discovered during implementation (via raw/ → ingest-source)
- Flag conflicts between established patterns and new requirements

---

## Contract

| Property | Value |
|----------|-------|
| Read path | `CLAUDE.md`, `wiki/INDEX.md`, `wiki/codebase-patterns.md` |
| Write path | `raw/YYYY-MM-DD-clawcode-adr-<slug>.md` |
| Format | Markdown — ADR format (context, decision, consequences) |
| Frequency | Read: every session. Write: after significant implementation. |
