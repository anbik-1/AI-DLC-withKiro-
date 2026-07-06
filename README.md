# AI-DLC-withKiro

Blueprint for running the **AI-Driven Development Lifecycle (AI-DLC)** methodology inside [Kiro CLI](https://github.com/aws/kiro), the AI assistant that guides a project from idea to deployed code through structured phases.

This repository is the *starting scaffold* — the rules, agents, and steering files that make Kiro follow the AI-DLC workflow. Real project code is generated *on top of* this scaffold during each Kiro session.

---

## What's in this repo

### `.kiro/` — the AI-DLC workflow blueprint

The core of the scaffold. It tells Kiro to follow the AI-DLC method (Inception → Construction → Operations) instead of ad-hoc coding.

- **`.kiro/steering/aws-aidlc-rules/core-workflow.md`** — the master workflow document. Defines the phases, stages, and gates. Loaded by Kiro at the start of every software-development request.
- **`.kiro/aws-aidlc-rule-details/`** — per-phase rule details Kiro loads as it moves through the workflow:
  - `common/` — cross-cutting rules (session continuity, content validation, question format, welcome message, process overview)
  - `inception/` — Workspace Detection, Reverse Engineering, Requirements Analysis, User Stories, Workflow Planning, Application Design, Units Generation
  - `construction/` — Functional Design, NFR Requirements, NFR Design, Infrastructure Design, Code Generation, Build & Test
  - `operations/` — placeholder for future deployment/monitoring workflows
- **`.kiro/agents/`** — agent configurations
- **`.kiro/settings/mcp.json`** — Model Context Protocol server config (empty by default)

### `labs/` — pre-existing sample exercises

Small standalone examples used for skills labs:

- `labs/snyk-lab/` — a small vulnerable Node.js app for security-testing exercises

### `sessions/` — Kiro session data

Populated by Kiro as you run project sessions. Empty in a fresh checkout.

---

## How to use this repo

### 1. Clone it

```bash
git clone https://github.com/anbik-1/AI-DLC-withKiro-.git
cd AI-DLC-withKiro-
```

### 2. Open in Kiro

Start Kiro CLI in this directory. Kiro will automatically detect `.kiro/steering/` and load the AI-DLC workflow rules.

### 3. Start a project

Ask Kiro to build something. For example:

> "Build an internal AI-powered proposal generator for our consulting team, with RAG over past proposals, Claude for drafting, and DOCX export."

Kiro will then walk through the AI-DLC phases:

1. **Workspace Detection** — greenfield vs brownfield check
2. **Requirements Analysis** — functional + non-functional requirements
3. **User Stories** (conditional) — for user-facing features
4. **Workflow Planning** — which stages to execute for this specific project
5. **Application Design** — components, services, method signatures, dependencies
6. **Units Generation** — decompose into buildable units
7. **Code Generation** — actual source code with tests
8. **Build & Test** — build instructions, unit/integration/perf/security/e2e test docs

At each gate Kiro pauses for your approval, so nothing surprising happens.

---

## Why AI-DLC

Ad-hoc "please build me X" prompts tend to produce inconsistent, half-baked code that skips security, testing, or architecture. AI-DLC forces a structured pass:

- Requirements are captured explicitly
- Architecture is documented before code is written
- Every unit has tests
- Deployment is planned, not improvised
- Every decision is auditable via `aidlc-docs/audit.md` (auto-generated per session)

---

## Repository structure

```
.
├── .kiro/                              # Kiro AI-DLC blueprint
│   ├── steering/aws-aidlc-rules/       #   → workflow rules
│   ├── aws-aidlc-rule-details/         #   → per-phase details
│   ├── agents/
│   └── settings/
├── labs/                               # Pre-existing lab exercises
│   └── snyk-lab/                       #   → vulnerable Node.js app
├── sessions/                           # Kiro session data (populated at runtime)
└── README.md                           # This file
```

---

## What gets generated during a session

When you run an AI-DLC session, Kiro creates additional folders that are **not** in this scaffold. Typical output for a full project:

```
├── aidlc-docs/                         # generated documentation
│   ├── audit.md
│   ├── aidlc-state.md
│   ├── inception/                      #   → requirements, user stories, app design
│   └── construction/                   #   → plans, per-unit code summaries, build & test
├── <your-app-folder>/                  # generated application code
│   ├── backend/
│   ├── worker/
│   ├── frontend/
│   └── infra/
└── (deploy runbooks, journals, etc.)
```

The scaffold repo intentionally stays *lean* — it's the reusable starting point, not the output.

---

## License

Internal use. Adapt as needed.
