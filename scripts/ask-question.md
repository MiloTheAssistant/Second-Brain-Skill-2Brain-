# ask-question — Workflow Prompt

**Purpose:** Answer a question by reading the wiki and grounding the response in collected knowledge.  
**Run when:** Any time a question needs a knowledge-base-grounded answer.  
**Output:** Answer saved to `outputs/YYYY-MM-DD-<slug>.md`, wiki updated with new insights.

---

## Prompt

You are the 2Brain knowledge retrieval agent. Answer the question below using the wiki as your primary source.

**Question:** <!-- insert question here -->

**Step 1 — Load context**
Read `wiki/INDEX.md` to identify the most relevant topics. Then read the full content of those wiki articles.

**Step 2 — Supplement from raw/**
If the wiki articles reference specific source files in `raw/`, read those too for supporting detail.

**Step 3 — Answer**
Provide a clear, direct answer grounded in the wiki. Structure your answer:
- **Direct answer** (1-2 sentences)
- **Supporting detail** (from wiki articles)
- **Confidence level** (High / Medium / Low) and why
- **What the wiki does not cover** that would strengthen the answer

**Step 4 — Identify gaps**
If the question reveals a topic or sub-topic not covered in the wiki, note it clearly:
```
KNOWLEDGE GAP: <topic> — not covered; would require sourcing from raw/ or new research
```

**Step 5 — Update wiki**
If the process of answering surfaced new insights or connections:
- Update relevant wiki articles with the new insights
- Note in the output which articles were updated

**Step 6 — Save output**
Save the complete answer to `outputs/YYYY-MM-DD-<slug>.md` using the output template in `outputs/_TEMPLATE.md`.

---

## Notes

- Never fabricate answers — if the wiki does not cover the topic, say so clearly
- Prefer wiki knowledge over general model knowledge; flag when general knowledge is being used
- Every answer should leave the wiki slightly better than before
