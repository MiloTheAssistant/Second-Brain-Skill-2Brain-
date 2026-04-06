# ingest-source — Workflow Prompt

**Purpose:** Add a single new source to `raw/` and immediately update the wiki.  
**Run when:** A new file or piece of content needs to be ingested right away (don't wait for compile-wiki).  
**Output:** Source in `raw/`, updated wiki articles, updated `wiki/INDEX.md`.

---

## Prompt

You are the 2Brain ingestion agent. Process the new source below and integrate it into the knowledge base.

**Source:** <!-- path to raw/ file, or paste content here -->

**Step 1 — Review the source**
Read the source fully. Identify:
- Main topics covered
- Key claims or facts
- Any named entities (people, organisations, tools, systems)
- Time-sensitivity (is this information that may expire or change?)

**Step 2 — Check for existing coverage**
Read `wiki/INDEX.md`. For each main topic in the source:
- Does a wiki article already exist?
- If yes: what does the existing article say? Does this source agree, add to, or conflict with it?

**Step 3 — Update or create wiki articles**
For each topic:
- **Existing article:** Add new information, mark conflicts with `[REVIEW: conflicts with <source>]`, do not delete existing content
- **New topic:** Create `wiki/<topic-name>.md` with one-paragraph summary, facts from source, cross-links to related topics
- Update `wiki/INDEX.md` for any new articles

**Step 4 — Report**
List:
- Articles created
- Articles updated (and what changed)
- Any conflicts with existing wiki content
- Follow-up: does this source suggest other topics that need research?

---

## Notes for Automated Ingestion (Composio MCP)

When Composio MCP ingests content from GitHub, Google Drive, or Slack:
- Content is saved to `raw/YYYY-MM-DD-<source>-<slug>.md` automatically
- Run this prompt on each new file to integrate it into the wiki
- For batch ingestion, use `scripts/compile-wiki.md` instead
