# Chisel

> Engineering-grade prompt architecture for AI agents.

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)
[![Skills](https://img.shields.io/badge/Skills-6%20stable%20%7C%201%20in%20development-blue)](skills/)
[![PRs Welcome](https://img.shields.io/badge/PRs-welcome-brightgreen.svg)](CONTRIBUTING.md)

Chisel is an open-source framework for turning rough, unstructured intent into prompts that AI agents can actually execute. It is built around the **Prompt Architecture Standard (PAS)** — a single, scalable framework that produces consistent results across creative writing, technical tasks, and high-stakes agentic workflows.

The structure stays the same. The depth scales with the task.

---

## Contents

- [Why Chisel](#why-chisel)
- [The Prompt Architecture Standard](#the-prompt-architecture-standard-pas)
- [Skills](#skills)
- [Installation](#installation)
  - [Claude Code](#claude-code)
  - [Claude Desktop & Claude.ai](#claude-desktop--claudeai)
  - [Claude Agent SDK](#claude-agent-sdk)
  - [Cursor](#cursor)
  - [Windsurf](#windsurf)
  - [Google Antigravity](#google-antigravity)
  - [Aider](#aider)
  - [Other agents (generic)](#other-agents-generic)
- [Usage example](#usage-example)
- [Roadmap](#roadmap)
- [FAQ](#faq)
- [Contributing](#contributing)
- [License](#license)
- [Author](#author)

---

## Why Chisel

Most developers are still figuring out how to work with AI agents. The default workflow — paste a rough thought into a chat box, hope the model understood — produces inconsistent results, wastes tokens, and makes "done" subjective.

Chisel is the framework built to stop the guessing. It is not a prompt library. It is a **standard** for turning rough intent into prompts that agents can execute, debug, and improve on. The structure is the same whether you are writing a birthday speech or refactoring a payment service — only the depth changes.

---

## The Prompt Architecture Standard (PAS)

PAS is the framework that powers every Chisel skill. It is ten sections, grouped into three tiers, applied in the same order every time.

### Tier 1 — Essential (every prompt)

| # | Section | Purpose |
|---|---|---|
| 1 | **Role** | Who the executor should act as. |
| 2 | **Objective** | The single goal, in one sentence. |
| 3 | **Context** | Background and the *why*. |
| 4 | **Instructions** | Ordered steps or substance of the request. |
| 5 | **Output Format** | Exact shape of the deliverable. |

### Tier 2 — Add for technical / agentic tasks

| # | Section | Purpose |
|---|---|---|
| 6 | **Scope** | What is explicitly in bounds. |
| 7 | **Constraints** | Hard `Do NOT` / `MUST` boundaries. |
| 8 | **Inputs** | Files, data, or prior material. |
| 9 | **Success Criteria** | Verifiable checklist for "done." |

### Tier 3 — Add for high-stakes / complex work

| # | Section | Purpose |
|---|---|---|
| 10 | **Validation & Reporting** | Self-check before finishing + completion-report format. |

Full specification: [`skills/prompt-architect/SKILL.md`](skills/prompt-architect/SKILL.md). Cheat sheet: [`skills/prompt-architect/quick-reference.md`](skills/prompt-architect/quick-reference.md).

---

## Skills

| Skill | Status | What it does |
|---|---|---|
| [`prompt-architect`](skills/prompt-architect/) | **Stable** | Converts rough prompts into structured PAS-format prompts. |
| [`frontend/next`](skills/frontend/next/) | **Stable** | Production-grade Next.js UI with opinionated, anti-AI-slop design rules. |
| [`frontend/react`](skills/frontend/react/) | **Stable** | Production-grade React UI (Vite, CRA, non-Next) — same design language as `frontend/next`. |
| [`frontend/flutter`](skills/frontend/flutter/) | **Stable** | Production-grade Flutter mobile UI (Material 3 + Cupertino, adaptive). |
| [`frontend/react-native`](skills/frontend/react-native/) | **Stable** | Production-grade React Native (Expo) mobile UI. |
| [`seo-next`](skills/seo-next/) | **Stable** | On-page + technical SEO for Next.js (App/Pages Router) — audits and implements to a 100/100 rubric. Pairs with `frontend/next`. |
| [`backend`](skills/backend/) | In development | Backend engineering patterns (APIs, services, data modeling). |

Each `frontend/*` skill:
- Includes a **pre-install questionnaire** (`questionnaire.md`) that calibrates output to your stack, brand, and skill level.
- Inherits the canonical design language from [`skills/frontend/shared-rules.md`](skills/frontend/shared-rules.md) — composition, brand, typography, color, hero discipline, motion, anti-patterns, litmus checks. Each sub-skill adds only platform-specific code patterns and idioms on top.

See [`skills/frontend/README.md`](skills/frontend/README.md) for an overview of the frontend skill family.

Every skill is plain markdown. There is no build step, no dependency graph, and no runtime requirement beyond the agent you choose to load it into.

---

## Installation

Every Chisel skill is a folder containing a `SKILL.md` file with YAML frontmatter (`name`, `description`) and a markdown body. How you load it depends on your agent runtime — pick the section that matches your stack.

### Claude Code

Claude Code auto-discovers skills in `.claude/skills/` (project) or `~/.claude/skills/` (user-level).

```bash
# Project-level (recommended for team repos)
mkdir -p .claude/skills
cp -r skills/prompt-architect .claude/skills/

# Or user-level (available across every project)
mkdir -p ~/.claude/skills
cp -r skills/prompt-architect ~/.claude/skills/
```

The skill becomes invokable via its description trigger, or explicitly:

> "Use the prompt-architect skill to clean up this prompt: …"

### Claude Desktop & Claude.ai

Claude's Skills feature accepts markdown-defined skills. In the Claude app or claude.ai web UI, open **Settings → Skills → Create skill** and upload the `skills/prompt-architect/` folder. The skill becomes available across all your conversations.

### Claude Agent SDK

Load the SKILL.md as your system prompt, or register it via the SDK's skill-loading API.

```python
from pathlib import Path
from anthropic import Anthropic

skill = Path("skills/prompt-architect/SKILL.md").read_text()

client = Anthropic()
response = client.messages.create(
    model="claude-opus-4-7",
    system=skill,
    messages=[{"role": "user", "content": "rough prompt to restructure..."}],
)
```

### Cursor

Cursor reads project rules from `.cursor/rules/`. Each rule is an MDC file with frontmatter.

```bash
mkdir -p .cursor/rules
cp skills/prompt-architect/SKILL.md .cursor/rules/prompt-architect.mdc
```

Then adjust the frontmatter to Cursor's MDC format:

```yaml
---
description: Convert rough prompts into structured PAS-format prompts.
globs:
alwaysApply: false
---
```

Invoke with `@prompt-architect` in chat, or let it auto-apply based on the description.

### Windsurf

Windsurf reads from `.windsurfrules` (single file in repo root) or `.windsurf/rules/` (multi-file).

```bash
mkdir -p .windsurf/rules
cp skills/prompt-architect/SKILL.md .windsurf/rules/prompt-architect.md
```

### Google Antigravity

Antigravity and other agents that follow the [AGENTS.md](https://agents.md) standard read instructions from `AGENTS.md` in the repo root.

```bash
# Reference the skill from AGENTS.md (lightweight)
echo "See skills/prompt-architect/SKILL.md for prompt architecture rules." >> AGENTS.md

# Or inline the skill content (heavier but self-contained)
cat skills/prompt-architect/SKILL.md >> AGENTS.md
```

Most AGENTS.md-aware agents will read referenced files on demand, so the lightweight reference is usually enough.

### Aider

Aider reads project conventions from `CONVENTIONS.md`.

```bash
cp skills/prompt-architect/SKILL.md CONVENTIONS.md
# Or append if you already have one:
cat skills/prompt-architect/SKILL.md >> CONVENTIONS.md
```

Recent Aider versions auto-detect `CONVENTIONS.md`; older versions need `aider --read CONVENTIONS.md`.

### Other agents (generic)

For any LLM playground, API, or custom agent that accepts a system prompt — including ChatGPT Custom GPTs, Gemini Gems, OpenAI Assistants, and raw API calls:

1. Open `skills/prompt-architect/SKILL.md`.
2. Copy the body (everything after the YAML frontmatter).
3. Paste it into your system prompt, custom instructions, or equivalent field.

The skill is self-contained markdown — no external dependencies, no runtime requirements.

---

## Usage example

**Input (rough prompt):**

> "hey can u fix my python script its too slow and also the variable names r bad. dont change what it does just make it faster and cleaner. also add comments"

**Output (structured prompt, PAS Tier 2):**

```markdown
# Optimize and Clean Up Python Script

## Role
You are a senior Python engineer focused on performance and readable code.

## Objective
Improve the runtime performance and readability of the provided Python script
without changing its behavior.

## Scope
The single attached Python script. Do not create new files.

## Constraints
- Do NOT change the script's observable behavior, inputs, or outputs.
- Do NOT add external dependencies unless strictly necessary (flag if so).
- MUST keep the script runnable as a drop-in replacement.

## Inputs
- The Python script provided in this conversation.

## Instructions
1. Profile or reason about the script to locate performance bottlenecks.
2. Optimize the slow sections while preserving identical behavior.
3. Rename unclear variables to descriptive, PEP 8-compliant names.
4. Add concise comments explaining non-obvious logic.

## Output Format
The full revised script in a single code block, followed by a short bullet
list of the changes made and why.

## Success Criteria
- [ ] Output is identical to the original for the same inputs.
- [ ] Measurable performance improvement.
- [ ] All variable names are descriptive and PEP 8-compliant.
- [ ] Non-obvious logic is commented.
```

The full skill includes more worked examples and edge-case handling — see [`skills/prompt-architect/SKILL.md`](skills/prompt-architect/SKILL.md).

---

## Roadmap

- [x] `prompt-architect` — core PAS skill (stable).
- [x] `frontend` — Next.js, React, Flutter, and React Native UI skills (stable).
- [x] `seo-next` — technical + on-page SEO for Next.js (stable).
- [ ] `backend` — API design, service boundaries, data modeling.

Have an idea for a new skill? [Open an issue](https://github.com/Devdannnny/chisel/issues).

---

## FAQ

**Is this just a prompt library?**
No. A prompt library hands you static prompts. Chisel hands you a *framework* — the same ten-section structure works for any task. You can write your own prompts in PAS format without using any of the example prompts in the repo.

**Why markdown? Why not a package or DSL?**
Every modern agent runtime accepts markdown. A DSL would require a parser, a build step, and version drift across tools. Markdown is the lowest-common-denominator format that still scales.

**Will this stay free?**
The framework and skills in this repo are MIT-licensed and will stay free. Future tooling on top (if any) may be separately licensed, but the core never closes.

**Can I use this commercially?**
Yes. MIT-licensed — use it, ship it, build on it. Attribution is appreciated but not required.

---

## Contributing

Contributions are welcome — especially new skills, refinements to PAS, and real-world examples. Open an issue before any non-trivial PR. See [`CONTRIBUTING.md`](CONTRIBUTING.md) for the full process.

---

## License

[MIT](LICENSE) © 2026 Daniel Dabiri.

---

## Author

**Daniel Dabiri** — Tech Co-Founder | AI/ML Engineer | Scaling businesses with Custom LLMs & Multi-Agent Systems

- GitHub: [@Devdannnny](https://github.com/Devdannnny)
- LinkedIn: [devdannny](https://www.linkedin.com/in/devdannny/)
- Email: [devdannny@gmail.com](mailto:devdannny@gmail.com)
