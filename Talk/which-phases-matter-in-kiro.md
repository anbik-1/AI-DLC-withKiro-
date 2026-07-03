# Which AI-DLC phases actually matter when using Kiro CLI?

**Date**: 2026-07-03
**Context**: The AI-DLC workflow defines 8 phases (Workspace Detection → Requirements → User Stories → Workflow Planning → Application Design → Units Generation → Code Generation → Build & Test). Not all of them add equal value in a solo-dev Kiro CLI session. This file is an honest phase-by-phase reality check.

---

## The question

> Does Workspace Detection / Requirements Analysis / User Stories really make sense when using Kiro CLI, where the AI is already conversational and can look at files on its own?

## Short answer

**Some phases carry their weight every time. Others are conditional and Kiro will (correctly) propose skipping them for simple projects.** The workflow is designed to be adaptive — you approve the phase list at the Workflow Planning stage, so nothing wasteful runs.

---

## Phase-by-phase honest evaluation

### 1. Workspace Detection
**What it does**: Scans the working directory, determines greenfield vs brownfield, records findings in `audit.md`.

**In Kiro CLI**: Kiro can already `ls` and read files on its own — no ceremony needed to figure out what's around.

**Value**: **Documentation, not discovery.** It's cheap and produces a baseline in `audit.md` that matters if you revisit the project months later.

- **Solo greenfield**: 30 seconds of theater. Fine, but low value.
- **Brownfield refactor**: **Deep and valuable** — the AI has to understand existing code before touching it.

**Verdict**: Keep it. It's short. Only meaningful for brownfield work.

---

### 2. Requirements Analysis
**What it does**: Asks clarifying questions, produces `requirements.md` with functional (FR) and non-functional (NFR) requirements.

**In Kiro CLI**: This is where the real conversation happens. When you describe a project, Kiro asks "auth? database? scale?" — that IS requirements analysis.

**Value**: **Huge.** This is the phase that saves you the most rework. Without it, the AI guesses what you meant and starts coding. With it, ambiguities surface *before* any code is written.

**Concrete example from Jin-Genese**: This phase forced clarification on:
- Auth: Cognito vs custom → Cognito
- DB: RDS vs DynamoDB → RDS
- Compute: Lambda vs Fargate → Fargate

Each of those would have been 2 hours of rework if the AI had guessed wrong.

**Verdict**: **Never skip this.** The `requirements.md` output becomes the ground truth for every downstream phase.

---

### 3. User Stories
**What it does**: Formalizes user-facing behavior as "As X, I want Y, so Z" stories. Defines personas.

**In Kiro CLI**: For a solo dev building an internal tool, this often feels like theater. You know the user (you). You know what you want.

**Value**: Real for **user-facing products with multiple personas** (customer + admin + agent) or **team collaboration** where BAs/PMs want to see stories. Overhead for solo internal tools.

**AI-DLC already knows this**: the `core-workflow.md` explicitly marks User Stories as **CONDITIONAL** and says to skip for internal refactors, simple bug fixes, or infra work.

**Verdict**: **Skip for simple / solo / internal projects.** Keep for user-facing multi-persona products.

---

### 4. Workflow Planning
**What it does**: Decides which downstream phases to execute + at what depth for THIS specific project.

**In Kiro CLI**: This IS where the "am I doing user stories or not?" decision gets made. It's the meta-phase that customizes the rest of the workflow to your project.

**Verdict**: **Always makes sense.** Cheap and important — it's what prevents you from wasting time on irrelevant phases.

---

### 5. Application Design
**What it does**: Components, services, method signatures, dependencies.

**In Kiro CLI**: For anything non-trivial, yes. Prevents the AI from painting itself into a corner during code generation.

**Value**: High for anything with more than one file. Low for tiny scripts.

**Verdict**: **CONDITIONAL** — skip for tiny scripts or single-file changes; keep for anything real.

---

### 6. Units Generation
**What it does**: Decomposes the system into buildable units of work.

**In Kiro CLI**: Only relevant for larger systems with multiple services / microservices.

**Verdict**: **CONDITIONAL** — skip for single-unit / monolithic apps; keep for microservices decomposition.

---

### 7. Code Generation
**What it does**: Writes the actual code, tests, docs.

**Verdict**: **Always.** This is why you're here.

---

### 8. Build & Test
**What it does**: Build instructions + unit / integration / performance / security / e2e test docs.

**Value**: Depends on how far you want to take it. For simple projects, unit tests are enough. For anything going to production, keep the full suite.

**Verdict**: **Always** at some depth. Match depth to production requirements.

---

## The reality-check matrix

| Phase | Solo dev, small tool | Internal tool for a team | User-facing product | Brownfield refactor |
|-------|:--:|:--:|:--:|:--:|
| 1. Workspace Detection | Light | Light | Light | **Deep** |
| 2. Requirements Analysis | **Do it** | **Do it** | **Do it** | **Do it** |
| 3. User Stories | Skip | Optional | **Do it** | Skip |
| 4. Workflow Planning | Always | Always | Always | Always |
| 5. Application Design | Optional | **Do it** | **Do it** | **Do it** |
| 6. Units Generation | Skip | Optional | **Do it** | Optional |
| 7. Code Generation | Always | Always | Always | Always |
| 8. Build & Test | Basic | Full | Full | Full |

---

## The workflow adapts — you don't have to fight it

At the **Workflow Planning** phase, Kiro proposes which phases to execute for your specific project. **You approve it.** So:

- **Solo internal tool?** Kiro proposes skipping User Stories + Units Generation. You approve, done.
- **Customer-facing product?** Kiro proposes the full set including User Stories with personas. You approve.
- **Small bug fix?** Kiro proposes skipping most inception phases entirely. You approve.

If Kiro proposes a phase that doesn't fit your project, say so:

> "Skip user stories — this is a solo internal tool."

Kiro will re-plan. The blueprint welcomes it.

---

## Practical guidance

1. **Never skip Requirements Analysis.** It's the highest-leverage phase in the workflow. Even 10 minutes of clarifying questions saves hours of rework.
2. **Workspace Detection is cheap** — let it run even when greenfield. It doesn't cost you much and gives you a clean baseline.
3. **User Stories are for user-facing products** with multiple personas. If it's just you using the app, tell Kiro to skip.
4. **Application Design proportional to complexity** — for a 3-file script, one paragraph of design is enough. For a full-stack app, do it properly.
5. **Build & Test always at some depth** — even a hobby project benefits from unit tests. Match the rigor to the deployment target.

---

## Bottom line

The AI-DLC methodology was designed to be **adaptive**, not ceremonial. The workflow rules explicitly mark half the phases as CONDITIONAL. Kiro proposes a per-project workflow at the Planning phase — you shape it to fit.

The phases you *always* keep, even in the smallest Kiro session:
- **Requirements Analysis** — the truth source for everything downstream
- **Workflow Planning** — decides what to keep/skip
- **Code Generation** — obvious
- **Build & Test** — at whatever depth matches your production target

The phases you're free to skip for solo / small work:
- User Stories
- Units Generation
- Deep Application Design (for tiny projects)
- Full Build & Test rigor (for hobby projects)

Kiro won't force ceremony on you. It'll propose a workflow, you'll shape it, then it executes.

See also:
- [`starting-a-new-project.md`](./starting-a-new-project.md) — the quickstart recipe
- [`ai-dlc-with-any-llm-provider.md`](./ai-dlc-with-any-llm-provider.md) — which LLMs work with this blueprint
- [`agentcore-runtime-vs-kiro-cli.md`](./agentcore-runtime-vs-kiro-cli.md) — why you don't need AgentCore for solo work
