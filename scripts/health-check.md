# health-check — Workflow Prompt

**Purpose:** Audit the knowledge base for quality issues.  
**Run when:** Monthly, or after large batches of new source material.  
**Output:** Health report saved to `outputs/YYYY-MM-DD-health-check.md`

---

## Prompt

You are the 2Brain health checker. Audit the wiki for quality and completeness.

**Step 1 — Contradiction scan**
Read all wiki articles. Flag any cases where two articles make conflicting factual claims. List each contradiction as:
```
CONFLICT: [[article-a]] says X, but [[article-b]] says Y — source: <raw file if known>
```

**Step 2 — Orphan topic scan**
Find topics that are mentioned via `[[cross-link]]` in wiki articles but do not have their own wiki file. List them as candidates for new articles.

**Step 3 — Unsourced claims**
Identify factual claims in wiki articles that have no corresponding source in `raw/`. List the top 5 unsourced claims by potential impact.

**Step 4 — Compounding error check**
Look for cases where an incorrect fact in one article may have propagated to others through cross-links. Flag any suspected chains.

**Step 5 — Gap analysis**
Based on what exists in the knowledge base, suggest 3 new wiki articles that would fill meaningful gaps. For each suggestion, note which existing articles or raw sources would feed it.

**Step 6 — INDEX.md check**
Verify that every wiki file has an entry in `INDEX.md`. List any missing entries.

**Step 7 — Report**
Save a report to `outputs/YYYY-MM-DD-health-check.md` using the output template. Include all findings with severity (HIGH / MEDIUM / LOW) and recommended actions.

---

## Severity Guide

| Severity | Meaning |
|----------|---------|
| HIGH | Contradictions or errors that could cause incorrect decisions |
| MEDIUM | Unsourced claims, orphaned topics, propagation risk |
| LOW | Missing INDEX entries, style inconsistencies, minor gaps |
