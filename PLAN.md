# 2Brain: Second Brain Knowledge Base System

## Context

This plan implements **2Brain** - a Karpathy-inspired Second Brain knowledge base skill that serves as the knowledge backbone connecting three systems in the ecosystem:

- **OpenClaw** (`github.com/MiloTheAssistant/OpenClawMaster`) - open-source autonomous AI agent framework; personal AI assistant running on any OS/platform via messaging interfaces; has a skill ecosystem via clawhub
- **Claw Code** (separate repo, being designed in another session) - code-focused AI development tool for the OpenClaw ecosystem
- **PaperClip** (`github.com/paperclipai/paperclip`) - open-source orchestration platform for autonomous AI agents; the "control plane for AI agents" with Node.js server + React UI; handles goal-aware execution, governance, multi-company support, cost tracking, and agent coordination

2Brain lives in its own repo (`Second-Brain-Skill-2Brain-`) and provides the shared knowledge infrastructure all three systems can read from and write to. The primary use case is **general knowledge** - a broad personal wiki covering multiple domains.

---

## Architecture Overview

```
Second-Brain-Skill-2Brain-/
├── CLAUDE.md                    # Schema + AI behavior rules
├── README.md                    # Project documentation (update existing)
├── raw/                         # Source material (unprocessed)
│   ├── .gitkeep
│   └── _INTAKE.md               # Instructions for adding raw sources
├── wiki/                        # AI-maintained organized knowledge
│   ├── INDEX.md                 # Master index of all topics
│   └── .gitkeep
├── outputs/                     # Generated reports, answers, analyses
│   ├── .gitkeep
│   └── _TEMPLATE.md             # Output template
├── scripts/                     # Automation and maintenance scripts
│   ├── compile-wiki.md          # Prompt: compile raw -> wiki
│   ├── health-check.md          # Prompt: run health check
│   ├── ask-question.md          # Prompt: query the knowledge base
│   └── ingest-source.md         # Prompt: ingest a new source into raw/
└── integrations/                # Integration configs for ecosystem
    ├── openclaw.md              # How OpenClaw connects to 2Brain
    ├── claw-code.md             # How Claw Code connects to 2Brain
    └── paperclip.md             # How PaperClip connects to 2Brain
```

---

## Implementation Steps

### Step 1: Update README.md
**File**: `README.md`

Rewrite with:
- Project overview and purpose
- Quick-start instructions (3-folder setup)
- How the compounding loop works
- Links to integration docs
- Attribution to Karpathy's approach

### Step 2: Create CLAUDE.md (Schema File)
**File**: `CLAUDE.md`

This is the core "one-file schema" that governs all AI behavior. Contents:
- **What This Is**: General-purpose personal knowledge base / Second Brain
- **Folder Rules**: raw/ is read-only source material; wiki/ is AI-maintained; outputs/ stores generated content
- **Wiki Rules**:
  - Every topic gets its own `.md` file in `wiki/`
  - Every wiki file starts with a one-paragraph summary
  - Link related topics using `[[topic-name]]` format
  - Maintain `INDEX.md` with every topic and one-line description
  - When new raw sources are added, update relevant wiki articles
  - Never delete wiki content - mark outdated info with `[OUTDATED]` and add corrections
- **Output Rules**: Save all generated answers/reports to `outputs/` with date-prefixed filenames
- **Health Check Rules**: Flag contradictions, unsourced claims, mentioned-but-unexplained topics
- **Integration Points**: How OpenClaw, Claw Code, and PaperClip interact with the knowledge base
- **Compounding Loop**: Outputs feed back into wiki updates; every interaction improves the system

### Step 3: Create Folder Structure
Create directories with `.gitkeep` files:
- `raw/.gitkeep`
- `wiki/.gitkeep`
- `outputs/.gitkeep`
- `scripts/` (no .gitkeep needed, will have files)
- `integrations/` (no .gitkeep needed, will have files)

### Step 4: Create raw/_INTAKE.md
**File**: `raw/_INTAKE.md`

Instructions for adding source material:
- Supported formats (.md, .txt, images, PDFs)
- Naming conventions (no need to organize - AI handles that)
- How to use agent-browser for automated collection
- Examples of good source material

### Step 5: Create wiki/INDEX.md
**File**: `wiki/INDEX.md`

Starter index template:
- Header explaining this is the AI-maintained master index
- Placeholder sections by domain/category
- Instructions that AI should update this file whenever wiki articles are added/modified

### Step 6: Create outputs/_TEMPLATE.md
**File**: `outputs/_TEMPLATE.md`

Template for generated outputs:
- Date, query/prompt, sources referenced
- Answer/report body
- Follow-up questions generated
- Wiki articles updated as a result

### Step 7: Create Workflow Prompts (scripts/)
Four prompt files that serve as reusable "skill invocations":

**scripts/compile-wiki.md** - The main compilation prompt:
- Read everything in `raw/`
- Create/update `wiki/INDEX.md`
- Create one `.md` per major topic
- Cross-link related topics
- Summarize every source

**scripts/health-check.md** - Monthly maintenance prompt:
- Flag contradictions between articles
- Find topics mentioned but never explained
- List claims not backed by sources in `raw/`
- Suggest 3 new articles to fill gaps
- Check for compounding errors

**scripts/ask-question.md** - Query prompt:
- Read across `wiki/` to answer questions
- Ground answers in collected material
- Save output to `outputs/`
- Update relevant wiki articles with new insights

**scripts/ingest-source.md** - Source ingestion prompt:
- Take a new file/URL and add to `raw/`
- Identify which wiki articles need updating
- Update `wiki/INDEX.md` if new topics emerge

### Step 8: Create Integration Configs (integrations/)

**integrations/openclaw.md** - OpenClaw integration:
- 2Brain as an OpenClaw skill (publishable to clawhub) - agents invoke it via messaging interface
- OpenClaw agents query the knowledge base for context before acting
- Agent conversation logs and outputs feed back into `raw/` as source material
- Skill definition format compatible with OpenClaw's skill ecosystem

**integrations/claw-code.md** - Claw Code integration:
- Claw Code sessions auto-load CLAUDE.md as project context
- Architecture decisions, debugging notes, and code patterns stored in wiki
- Code review insights and refactoring decisions feed back into the knowledge base
- Shared CLAUDE.md conventions between 2Brain and Claw Code repos

**integrations/paperclip.md** - PaperClip integration:
- PaperClip orchestrates 2Brain as one of its managed agents
- PaperClip's goal-aware execution triggers wiki compilation and health checks on schedule
- Agent coordination: PaperClip routes knowledge queries to 2Brain, routes agent outputs back as raw sources
- Cost tracking: PaperClip monitors token usage for 2Brain compilation/query operations
- Governance: PaperClip enforces review gates before wiki articles are finalized

### Step 9: Commit and Push
- Stage all new/modified files
- Commit with descriptive message
- Push to `claude/second-brain-knowledge-base-u49dM`

---

## Verification

1. **Structure check**: Verify all folders and files exist with correct paths
2. **Schema check**: Read CLAUDE.md and confirm it covers all rules from Karpathy's approach
3. **Completeness check**: Ensure INDEX.md, templates, and all prompts are in place
4. **Integration check**: Confirm each integration doc references the correct external repos
5. **Git check**: Verify clean commit on the correct branch, pushed to remote

---

## Key Design Decisions

- **Flat file structure**: Following Karpathy's "super simple and flat" philosophy - no databases, no plugins, just markdown files and folders
- **Prompt-as-script**: Workflow prompts in `scripts/` are markdown files containing the prompts to run, not executable code - keeping everything tool-agnostic
- **Integration docs as contracts**: Each integration file defines the interface between 2Brain and the external system, serving as a contract for the other repos to implement against
- **Compounding by design**: Every interaction (query, health check, compilation) is designed to leave the knowledge base better than it found it
- **2Brain as the shared memory layer**: OpenClaw provides the agent skills, PaperClip provides the orchestration/governance, Claw Code provides the development environment — 2Brain provides the persistent knowledge that all three share
