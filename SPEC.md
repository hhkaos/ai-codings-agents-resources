
# ai-templates

> A public, agent-agnostic collection of skills, templates, and guides for developers who code with AI agents.

---


<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Problem](#problem)
- [What this is](#what-this-is)
- [Core features (v1)](#core-features-v1)
- [Key user flows](#key-user-flows)
  - [Setting up an agent on a new project](#setting-up-an-agent-on-a-new-project)
- [Out of scope (v1)](#out-of-scope-v1)
- [Tech constraints](#tech-constraints)
- [Success criteria](#success-criteria)
- [Open questions](#open-questions)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Problem

Developers waste time searching for resources that aren't explained for their specific context: their agent, their language, their framework. Every new project means setting everything up from scratch. There's no reusable, neutral reference point that works regardless of which AI agent they use.

---

## What this is

A documentation repository that centralizes reusable skills, context file templates, and workflow guides for AI coding agents. It's designed for any developer, though the initial focus is developers using Esri technologies (ArcGIS Maps SDK, Calcite Design System, etc.). All content is written in an agent-agnostic way: the developer copies what they need and adapts it to their environment with no install or configuration step required.

**Language:** All repo content is written in English, regardless of the language used in conversations or issues.

---

## Core features (v1)

- **Skills** — reusable instructions that developers copy into their project as slash commands or prompt recipes (`init-spec`, `review-spec`, `init-memory`, `ship`)
- **Templates** — ready-to-adapt context files per agent (`CLAUDE.md`, `AGENTS.md`, `copilot-instructions.md`, etc.)
- **Guides** — workflow pattern documentation, git conventions, and setup checklists for new projects

---

## Key user flows

### Setting up an agent on a new project

1. Developer opens the repo and browses `skills/` or `agents/`
2. Finds the skill or template they need
3. Copies it into their project at the correct path for their agent
4. Adapts the content to their project
5. If they have questions or suggestions, they open a GitHub issue

---

## Out of scope (v1)

- No tooling, no npm package, no CLI
- No build step or CI/CD
- No automatic file generation
- The copy process is entirely manual

---

## Tech constraints

- Markdown files only — no dependencies
- Deliberate folder structure: every skill has `prompt.md` + `README.md`
- Skill content must be agent-agnostic

---

## Success criteria

- Covers the 3 main agents: **Claude Code**, **GitHub Copilot**, and **OpenAI Codex**
- A developer can set up their agent on a new project in under 10 minutes using this repo as a reference

---

## Open questions

- [ ] Does it make sense to build a CLI in the future that automates copying skills based on the agent being used, instead of doing it manually?
