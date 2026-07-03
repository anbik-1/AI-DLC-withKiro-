# Can I use AI-DLC with Kiro and any LLM provider?

**Date**: 2026-07-03
**Context**: After cleaning up the Jin-Genese deploy and reducing the repo back to just the AI-DLC blueprint scaffold, the question came up: is this approach tied to Anthropic Claude, or can it work with any LLM?

---

## The question

> My question is — can I use this AI-DLC concept with Kiro and the LLM model of any provider?

## Short answer

**Yes, at two levels.**

1. Kiro CLI itself already supports multiple providers — you can switch models mid-session.
2. The AI-DLC concept (the `.kiro/` steering files) is just markdown, so it's portable to other AI coding tools if you want to leave Kiro.

---

## Level 1 — Kiro CLI supports multiple providers

Verified against the Kiro CLI documentation. The `--model` flag and `/model` slash command let you switch models from different providers within the same session.

### Model families documented in Kiro

| Provider | Example model IDs |
|----------|-------------------|
| **Anthropic** | `claude-opus-4.7`, `claude-sonnet-4.5` |
| **OpenAI** | `openai-gpt-5.4`, `openai-gpt-5.4-1p`, `openai-gpt-5.5-1p`, `openai-gpt-oss-20b` |
| **Google** | `gemini-2.5-pro` |
| **Moonshot** | `kimi-k2.5` |
| **NVIDIA** | `nemotron-super-3-120b` |
| **MiniMax** | `minimax-*` |
| **Zhipu** | `glm-*` |

### How to switch models in Kiro

Start a session with a specific model:
```bash
kiro-cli chat --model openai-gpt-5.5-1p "Build a Flask API for a todo list"
```

Switch mid-session with a slash command:
```
/model
```
This opens a picker and shows available models.

### Key point

The AI-DLC steering rules in `.kiro/` load regardless of which model you pick. A session that starts with Claude and switches to GPT will still follow the same phases → gates → approvals → code generation flow. The workflow is decoupled from the model.

---

## Level 2 — AI-DLC itself is provider-agnostic

The `.kiro/` folder is just markdown files describing a workflow. Nothing in it is Anthropic- or AWS-specific:

- `.kiro/steering/aws-aidlc-rules/core-workflow.md` — "phase 1 do X, ask for approval, phase 2 do Y..."
- `.kiro/aws-aidlc-rule-details/construction/code-generation.md` — "generate files under this structure, run these tests..."
- etc.

Any capable LLM that can:
- Follow multi-step structured instructions
- Read and write files
- Run shell commands / tests
- Use tools

...can execute AI-DLC.

### Beyond Kiro CLI

The steering files can be adapted to other AI coding tools that support project-level rules:

- **Cursor** — copy into `.cursorrules` or `.cursor/rules/*.mdc`
- **Cline / Roo Code** (VS Code) — reference the files in the system prompt
- **Aider** — pass via `--message-file` or a `CONVENTIONS.md`
- **Claude Desktop with MCP** — expose them via a filesystem MCP server
- **Continue.dev** — put them in `.continue/rules/`

---

## Caveats

1. **Model quality matters.** AI-DLC has many gates and expects the model to generate large, structured artifacts (requirements docs → code → tests). Weaker or older models (small open-source, GPT-3.5) will struggle. Claude Sonnet/Opus 4.x, GPT-5.x, and Gemini 2.5 Pro all handle it fine.
2. **Tool ecosystem matters.** The workflow depends on the assistant being able to actually execute — read/write files, run tests, invoke shell commands. Chat-only interfaces (raw ChatGPT web without Code Interpreter) can produce the artifacts but can't run them.
3. **Reformat when switching tools.** Kiro reads `.kiro/steering/*.md` automatically. Cursor reads `.cursorrules`. Cline reads `.clinerules`. The *content* is portable; the *convention* about where to put it isn't.

---

## Recommendation

- **Easiest path**: stay inside Kiro CLI and use `/model` to try different providers. Zero friction.
- **If moving to another tool**: copy `.kiro/steering/aws-aidlc-rules/core-workflow.md` and the relevant `.kiro/aws-aidlc-rule-details/*` files into that tool's rule directory, then reference them in your system prompt.

The blueprint travels with you.
