# Starting a new project with this blueprint

**Date**: 2026-07-03
**Context**: After the earlier Q&As on model portability and AgentCore-vs-Kiro, the natural next question — how do I actually kick off a project using what's in this repo?

---

## The question

> Suppose I need to build any project — then simply I can start with the blueprint of the AI-DLC in here and any LLM with Kiro CLI?

## Short answer

**Yes, exactly that.** The blueprint + Kiro CLI + any capable LLM is a complete AI-DLC setup. Clone, start Kiro, describe the project, hit enter.

---

## The 4-step recipe

### 1. Clone the blueprint
```bash
git clone https://github.com/anbik-1/AI-DLC-withKiro-.git my-new-project
cd my-new-project
```

### 2. Start Kiro CLI in that directory
```bash
kiro-cli chat
```

Kiro auto-detects `.kiro/steering/` and loads the AI-DLC workflow rules.

### 3. (Optional) Pick your model
```
/model
```

Pick Claude Opus 4.7, GPT-5.x, Gemini 2.5 Pro, or whichever capable model you prefer. For serious work stick with the strongest available — smaller models drop details across the many AI-DLC gates.

### 4. Describe your project
```
> Build an internal expense tracker for our team, with receipt OCR, monthly reports, and Slack notifications.
```

That's it. Kiro walks through the AI-DLC phases automatically:

| # | Phase | Always runs? | Real value for a solo dev |
|---|-------|:--:|--|
| 1 | Workspace Detection | Yes | Low for greenfield (records baseline), high for brownfield |
| 2 | Requirements Analysis | Yes | **Highest** — surfaces ambiguity before any code is written |
| 3 | User Stories | Conditional | Skip for solo / internal; keep for user-facing products |
| 4 | Workflow Planning | Yes | Decides which phases to skip for THIS project |
| 5 | Application Design | Conditional | Skip for tiny scripts; keep for multi-file apps |
| 6 | Units Generation | Conditional | Skip for monoliths; keep for microservices |
| 7 | Code Generation | Yes | The main event |
| 8 | Build & Test | Yes | Depth scales with production requirements |

**The workflow is adaptive.** At the Workflow Planning phase, Kiro proposes which phases to include for your specific project. You approve or amend. If a phase doesn't fit ("skip user stories, this is a solo tool"), tell Kiro and it re-plans.

At every gate Kiro pauses for your approval. You review, refine, then approve to move to the next phase.

See [`which-phases-matter-in-kiro.md`](./which-phases-matter-in-kiro.md) for the honest phase-by-phase evaluation.

---

## What you get out of the box

- Every project starts from the same disciplined baseline: requirements → design → tests → deploy
- Full **audit trail** auto-written to `aidlc-docs/audit.md`
- Generated app code lives alongside the blueprint (or you can move it to a separate repo)
- Structured docs in `aidlc-docs/inception/` and `aidlc-docs/construction/`

---

## Practical tips

1. **Use a strong model.** Claude Opus / GPT-5 / Gemini 2.5 Pro. AI-DLC has many multi-step gates that weaker models handle sloppily.
2. **Approve deliberately, not blindly.** At each gate Kiro shows you what it's about to do. Read it. Reject or refine if wrong. That's the whole point of the workflow.
3. **The blueprint stays lean.** Everything generated goes into `aidlc-docs/` + a project folder. Delete both when the project is done, or push them to a separate repo. The blueprint scaffold itself stays reusable.
4. **API keys as needed.** If the project needs Anthropic, Cohere, Tavily, OpenAI, etc., have those keys ready. If it needs AWS deployment, have credentials configured.
5. **Time expectation.** Realistic full-stack app with tests + deploy = 1-2 workdays of session time. Simple internal tool with no infra = 2-4 hours.
6. **Iterate on the blueprint.** After each project, if you find yourself repeatedly correcting the same thing at some phase, edit the corresponding `.kiro/aws-aidlc-rule-details/*.md` file so future projects don't need that correction. The blueprint gets sharper with each use.

---

## What you don't need

- ❌ AWS account (unless the project deploys to AWS)
- ❌ AgentCore Runtime
- ❌ Strands framework
- ❌ Any other AI-DLC-related AWS product
- ❌ A team — solo dev is the sweet spot

---

## Alternatives to Kiro CLI

If you prefer another AI coding tool, the blueprint is portable — the `.kiro/` folder is just markdown. Adapt to:

- **Claude Code** (Anthropic's CLI) — reference the steering files in your system prompt
- **OpenCode** — same approach
- **Cursor** — copy content into `.cursorrules` or `.cursor/rules/*.mdc`
- **Cline / Roo Code** (VS Code) — reference in system prompt
- **Aider** — pass via `--message-file`
- **Continue.dev** — put in `.continue/rules/`

The methodology travels; only the tool-specific rule-file convention changes.

---

## Bottom line

You already have everything. The blueprint is in this repo. Kiro is installed. Pick a strong model. Describe the project. Hit enter. Follow the gates. You'll get a production-quality result from a disciplined process, without over-engineering.

See also:
- [`which-phases-matter-in-kiro.md`](./which-phases-matter-in-kiro.md) — honest phase-by-phase evaluation (which to keep, which to skip)
- [`ai-dlc-with-any-llm-provider.md`](./ai-dlc-with-any-llm-provider.md) — which LLMs work with this blueprint
- [`agentcore-runtime-vs-kiro-cli.md`](./agentcore-runtime-vs-kiro-cli.md) — why you don't need AgentCore for solo work
