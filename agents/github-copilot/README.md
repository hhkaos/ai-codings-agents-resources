# GitHub Copilot Setup Guide

GitHub Copilot reads repository-scoped instructions from a dedicated markdown file. It also supports custom prompt files that can be invoked as slash commands in Copilot Chat.

---

## Project context file: `copilot-instructions.md`

Place your instructions at:

```
.github/copilot-instructions.md
```

This file is automatically read by Copilot in VS Code, JetBrains, and GitHub.com. No front matter needed — plain markdown.

**What to include:**
- Project overview
- Tech stack and key libraries
- Coding conventions
- Things Copilot should always or never do
- Security rules (files not to touch, staging policy)

**Example opening:**
```markdown
Instructions for GitHub Copilot in this repository.

## Project

[2–3 sentences: what it does, who it's for, key technical approach.]

## Stack

| Concern | Choice |
|---|---|
| Language | TypeScript |
| Framework | React 18 |
| Bundler | Vite |

## Coding style

- No TypeScript `any` — use proper types or `unknown`.
- Co-locate CSS with its component.
- Named exports only — no default exports.

## Never do

- Never commit `.env` files or private keys.
- Never use `git add -A` — stage files by name.
```

Use the `/init-memory` skill (see [skills/init-memory](../../skills/init-memory/)) to generate this file interactively alongside your other agent context files.

---

## Prompt files (custom slash commands)

Copilot Chat supports custom prompt files that appear as `/` commands:

```
.github/prompts/<name>.prompt.md
```

**Front matter required:**
```yaml
---
mode: agent
description: Short description shown in the command picker.
---
```

**Modes:**
| Mode | Behaviour |
|---|---|
| `ask` | Answers questions in chat |
| `edit` | Edits files directly |
| `agent` | Full agent mode — can read files, run commands, create files |

**To install a skill from this repo:** copy the content of `skills/<skill-name>/prompt.md` into `.github/prompts/<skill-name>.prompt.md` and add the front matter above.

---

## Available skills

| Skill | Install path | Invoke with |
|---|---|---|
| [init-memory](../../skills/init-memory/) | `.github/prompts/init-memory.prompt.md` | `/init-memory` in Copilot Chat |
| [review-spec](../../skills/review-spec/) | `.github/prompts/review-spec.prompt.md` | `/review-spec` in Copilot Chat |

---

## Tips

- Copilot does not have an `AskUserQuestion` tool — skills that need interactive input will ask questions as plain text. The "When you need information from me" block in each skill handles this.
- Keep `copilot-instructions.md` under ~8,000 tokens — Copilot truncates beyond that.
- For larger projects, split concerns across multiple prompt files rather than one giant instructions file.
