# AgentCore Runtime vs Kiro CLI — do I need both?

**Date**: 2026-07-03
**Context**: Reviewing a workshop diagram from the AWS team showing an AI-DLC architecture built on top of AgentCore Runtime, with specialized agents for each phase (Orchestrator, Workspace Detection, Requirements Analysis, User Stories, etc.), all using Strands agents + AgentCore Observability. Question: if I can already do everything through Kiro CLI with LLM access, do I need this whole AgentCore setup?

---

## The question

> There is a workshop provided by the AWS team where I can see AgentCore Runtime, below it an AI-DLC Orchestrator Agent, Workspace Detection Agent, Requirements Analysis Agent, User Stories Agent, Strands Agents, AgentCore Observability. Now my question is — if I am doing everything through the Kiro CLI with access to the LLM model, it can pretty much follow the rules in Inception, Construction, Operations and do anything, right?

## Short answer

**Yes, you are 100% right.** Kiro CLI with an LLM and the `.kiro/` blueprint can execute every phase of AI-DLC end-to-end. The AgentCore multi-agent version is the **same methodology packaged differently** — for a different audience and scale.

---

## Two implementations of the exact same methodology

### Path A — Kiro CLI (what we've been doing)
- **One model, one conversation, one project**
- Steering files (`.kiro/steering/*.md`) guide behavior
- Human at the keyboard, approving each gate
- All context lives in the chat history
- Runs on your machine or workshop instance
- Free (or subscription cost)

### Path B — Multi-agent on AgentCore (what the workshop shows)
- **Many specialized agents**, each handling one phase:
  - Orchestrator agent → dispatches work
  - Workspace Detection agent → analyzes existing code
  - Requirements Analysis agent → generates functional/NFR docs
  - User Stories agent → produces stories + personas
  - (Application Design agent, Code Gen agent, etc.)
- Built with **Strands** (AWS's agent framework)
- Hosted on **AgentCore Runtime** (managed compute per session)
- **AgentCore Observability** for tracing what each agent did
- Runs as an AWS service, not on your laptop

---

## Why would anyone build the multi-agent version at all?

Not because Kiro can't do it. Because the packaging solves different problems.

| Concern | Kiro CLI | Multi-agent on AgentCore |
|---------|----------|-------------------------|
| Who's the user? | A developer | A whole team (BAs, PMs, engineers) or an external customer |
| Where does it run? | Your machine | Managed AWS service, always on |
| Concurrent projects? | One at a time | Many, isolated per session |
| Human oversight? | Every step | Can run unattended for long stretches |
| Model per phase? | One model for everything | Cheap model for detection, expensive Claude for code gen |
| Memory | Chat history only | Persistent memory across sessions (AgentCore Memory) |
| Observability | Terminal output | Traces, session replay, evaluations across all runs |
| Delivery | Dev tool | SaaS-shaped product |
| Cost | Free / low | Real AWS bill per session |

---

## When each approach wins

### Kiro CLI (or Claude Code, OpenCode, Cursor) wins when
- You're a developer using AI-DLC to build one project at a time
- You want to see every file, approve every action
- You need to iterate quickly — pause, tweak, re-run
- You don't want to pay AWS for hosted infrastructure
- You want the blueprint to travel with you (git-clone-and-go)

### AgentCore multi-agent wins when
- You want to offer AI-DLC as a **service** to your organization
- Multiple teams run projects **in parallel** without stepping on each other
- You want **audit trails** across all AI-DLC runs your company has ever done
- **Non-developers** should be able to kick off a project (they'd be lost in a CLI)
- You need to **swap models per phase** — GPT-mini for workspace detection, Claude Opus for code generation
- You need **compliance-grade isolation** — each session in its own sandbox

---

## Analogy

- **Kiro CLI approach** = hiring a consultant who works at *your* desk with *you*. They follow a methodology (AI-DLC), but you're always in the room.
- **AgentCore multi-agent approach** = building a *consulting firm* that other people can hire. Same methodology, but industrialized. Their own office, their own audit trail, their own compliance guarantees.

Neither is better in absolute terms. They're the same idea at different scales.

---

## Summary — Can a solo dev do a full project with just LLM + Kiro/Claude Code/OpenCode + AI-DLC blueprint?

**Yes. Without issues.** This is the specific scenario Kiro CLI is designed for.

Here's what you actually need and what you don't:

### What you need
1. **An LLM with tool-use capability** — Claude Sonnet/Opus 4.x, GPT-5.x, or Gemini 2.5 Pro. The Kiro CLI, Claude Code CLI, or OpenCode delivers this.
2. **The AI-DLC blueprint** — the `.kiro/` folder in this repo. It's just markdown; no runtime required.
3. **A working directory** — where the AI will read/write files.
4. **Your time and attention** — you'll be approving gates every 5-15 minutes.

### What you DON'T need
- ❌ AgentCore Runtime
- ❌ Strands agent framework
- ❌ AgentCore Observability, Memory, or Gateway
- ❌ Any AWS account (unless the project you're building itself needs AWS resources)
- ❌ Multiple specialized agents
- ❌ A production orchestration layer

### Concrete proof
The Jin-Genese project we just built + deployed + tore down was done exactly this way:
- One Kiro CLI session
- One LLM (Claude Opus 4.7)
- The `.kiro/` blueprint driving the workflow
- 21-step code generation plan executed against my working directory
- Full-stack app: Python backend + worker + React frontend + AWS CDK infra + tests + deploy
- Deployed to real AWS Fargate, tested end-to-end
- Everything in about 6 hours of session time

No AgentCore. No Strands. No multi-agent orchestration. Just one model following the blueprint step by step.

### The one honest caveat

The **quality of the output** scales with the **capability of the model**. If you use Claude Opus / GPT-5 / Gemini 2.5 Pro, you'll get production-quality output. If you use a small open-source model or GPT-3.5, the same blueprint will produce something less polished — the model won't follow the multi-step gates as carefully and may skip parts of the code generation.

Kiro CLI lets you pick which model runs (see `Talk/ai-dlc-with-any-llm-provider.md`). Pick a strong one and you're set.

### Bottom line

For a solo developer or small team doing real project work: **you have everything you need already**. The blueprint + Kiro CLI + a strong LLM is a complete AI-DLC setup. AgentCore is what you'd add later if you wanted to productize AI-DLC into a service other people can use.

Don't over-engineer. Start with Kiro. Add AgentCore only when the pain of "everyone in my company keeps asking me to run AI-DLC for them" becomes real.
