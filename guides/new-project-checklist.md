
# New Project Setup Checklist

Use this checklist when starting a new project with AI coding agent support.

> Not all steps apply to all projects — pick what makes sense. The core value is in: a good context file (`CLAUDE.md`/`AGENTS.md`), file-specific git staging, test gates, and AI commit tracking.


<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->
**Table of Contents**  *generated with [DocToc](https://github.com/thlorenz/doctoc)*

- [Initial Setup](#initial-setup)
  - [1. Repository Setup](#1-repository-setup)
  - [2. Project Structure](#2-project-structure)
  - [3. Git Configuration](#3-git-configuration)
  - [4. Package Configuration](#4-package-configuration)
  - [5. Git Hooks](#5-git-hooks)
  - [6. Testing Setup](#6-testing-setup)
  - [7. Documentation Structure](#7-documentation-structure)
  - [8. CI/CD Setup](#8-cicd-setup)
  - [9. CLAUDE.md Completion](#9-claudemd-completion)
  - [10. Custom Skills Setup (Claude Code)](#10-custom-skills-setup-claude-code)
- [Development Phase](#development-phase)
  - [First Feature](#first-feature)
  - [Security Review](#security-review)
  - [Deployment Checklist](#deployment-checklist)
- [Quick Reference](#quick-reference)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Initial Setup

### 1. Repository Setup
- [ ] Create new repository
- [ ] Initialize with README.md
- [ ] Copy and customize [`agents/claude-code/CLAUDE.md.template`](../agents/claude-code/CLAUDE.md.template) → `CLAUDE.md`
- [ ] Run `/init-memory` to generate context files for other agents if needed

### 2. Project Structure
- [ ] Decide: Single app or monorepo?
- [ ] Create directory structure:
  ```
  project-root/
  ├── app1/                 # Main app
  ├── app2/                 # Admin app (if needed)
  ├── docs/
  │   ├── SPEC.md
  │   ├── TODO.md
  │   └── CHANGELOG.md
  ├── package.json          # Root scripts
  ├── CLAUDE.md
  ├── .gitignore
  └── README.md
  ```

### 3. Git Configuration

**Set up aliases** (run once globally or per-repo):
```bash
git config --global alias.ch '!f() { git commit -m "$1"; }; f'
git config --global alias.cai '!f() { git commit -m "AI: $1" --author="AI Assistant <ai@example.com>"; }; f'
```

- `git ch "message"` — human commit
- `git cai "message"` — AI-authored commit (makes AI contributions visible in git log)

**Create `.gitignore`**:
```
# Dependencies
node_modules/

# Build
dist/
build/
.vite/

# Security
*.pem
*.key
.env
.env.local
.env.production
credentials*.json

# OS
.DS_Store
Thumbs.db

# Test
coverage/
.nyc_output/
```

### 4. Package Configuration

**Root `package.json`** (monorepo):
```json
{
  "name": "project-name",
  "version": "1.0.0",
  "private": true,
  "workspaces": ["app1", "app2"],
  "scripts": {
    "dev": "npm run dev --workspace=app1 & npm run dev --workspace=app2",
    "test": "npm test --workspace=app1 && npm test --workspace=app2",
    "lint": "npm run lint --workspaces"
  },
  "devDependencies": {
    "husky": "^9.0.0"
  }
}
```

Key patterns:
- `&` for parallel execution (dev servers)
- `&&` for sequential execution (tests — fail-fast)
- `--workspace` to target a specific app
- `--workspaces` to run across all

### 5. Git Hooks

**Pre-push hook** (`.husky/pre-push`):
```bash
#!/bin/sh
npm test
```

Tasks:
- [ ] Install Husky: `npm install --save-dev husky`
- [ ] Initialize: `npx husky init`
- [ ] Create `.husky/pre-push` with the above content
- [ ] Make executable: `chmod +x .husky/pre-push`
- [ ] Test: try `git push` and verify tests run

### 6. Testing Setup

**`vite.config.js`** (per app):
```javascript
import { defineConfig } from 'vite';

export default defineConfig({
  test: {
    globals: true,
    environment: 'jsdom',
    setupFiles: './src/test-setup.js',
    coverage: {
      provider: 'v8',
      reporter: ['text', 'html']
    }
  }
});
```

Tasks:
- [ ] Install: `npm install --save-dev vitest jsdom @testing-library/dom`
- [ ] Add `"test": "vitest run"` and `"test:watch": "vitest"` to each app's package.json
- [ ] Write an initial smoke test to verify the setup works

### 7. Documentation Structure

- [ ] Create `docs/SPEC.md` — technical specification and requirements
- [ ] Create `docs/TODO.md` — task backlog
- [ ] Create `docs/CHANGELOG.md`:
  ```markdown
  # Changelog

  ## [Unreleased]
  ### Added
  ### Changed
  ### Fixed
  ```

### 8. CI/CD Setup

**Test workflow** (`.github/workflows/test.yml`):
```yaml
name: Tests

on: [push, pull_request]

jobs:
  test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: '20'
      - run: npm install && npm install --workspaces
      - run: npm test
```

**GitHub Pages — multi-app monorepo** (deploy app1 at `/`, app2 at `/app2/`):

Each app's `vite.config.js`:
```javascript
// app1
export default { base: '/' }

// app2
export default { base: '/app2/' }
```

Combine artifacts before deploying:
```bash
mkdir -p dist
cp -r app1/dist/* dist/
mkdir -p dist/app2
cp -r app2/dist/* dist/app2/
```

### 9. CLAUDE.md Completion

Fill in all sections:
- [ ] Project overview (purpose + tech approach)
- [ ] Commands (with ports and output locations)
- [ ] Architecture (structure + data flow)
- [ ] Deployment (platform + URL structure + CI/CD)
- [ ] Git conventions (commits + skills + rules)
- [ ] Security rules (what never to commit)

### 10. Custom Skills Setup (Claude Code)

- [ ] Copy skills from [`skills/`](../skills/) into `.claude/commands/`
- [ ] Add `/ship` skill — commit + push + update CHANGELOG/TODO (see [`skills/`](../skills/))
- [ ] Add `/release` skill — version bump + tag + GitHub Release
- [ ] Document installed skills in `CLAUDE.md`

---

## Development Phase

### First Feature
- [ ] Create feature branch: `git checkout -b feat/initial-setup`
- [ ] Write feature with tests
- [ ] Run tests: `npm test`
- [ ] Run linter: `npm run lint`
- [ ] Commit: `git ch "feat: add initial feature"`
- [ ] Push and verify CI passes

### Security Review
- [ ] Review `.gitignore` for sensitive patterns
- [ ] Confirm file-specific staging is the team habit (never `git add -A`)
- [ ] Verify no secrets in code or localStorage
- [ ] Test that sensitive file patterns are blocked by git

### Deployment Checklist
- [ ] Build locally: `npm run build --workspaces`
- [ ] Test the build locally
- [ ] Configure deployment platform
- [ ] Deploy and verify all routes/subpaths work

---

## Quick Reference

**Key files**:
- `CLAUDE.md` — AI guidance and project overview
- `docs/SPEC.md` — technical specification
- `docs/TODO.md` — task backlog
- `docs/CHANGELOG.md` — release history
- `.husky/pre-push` — test enforcement
- `.github/workflows/` — CI/CD

**Key commands**:
```bash
npm run dev              # Start dev servers
npm test                 # Run all tests
npm run lint             # Lint all apps
git ch "message"         # Human commit
git cai "message"        # AI commit
```

**✅ DO**
- Stage files by name: `git add src/file.js docs/CHANGELOG.md`
- Use conventional commits: `feat:`, `fix:`, `docs:`, `refactor:`, `chore:`
- Track AI commits with `git cai` so they're visible in git log
- Update CHANGELOG.md with each feature

**❌ DON'T**
- Never `git add -A` or `git add .`
- Never commit `*.pem`, `*.key`, `.env`, credentials
- Never store private keys in localStorage/sessionStorage
- Never skip pre-push tests
