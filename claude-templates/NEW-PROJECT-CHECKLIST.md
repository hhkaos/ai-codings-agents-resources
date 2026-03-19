# New Project Setup Checklist

Use this checklist when starting a new project based on the Digital Membership Card System patterns.

## Initial Setup

### 1. Repository Setup
- [ ] Create new repository
- [ ] Initialize with README.md
- [ ] Copy and customize `CLAUDE.md.template` → `CLAUDE.md`
- [ ] Review `PROJECT-WORKFLOW-PATTERNS.md` for applicable patterns

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
  │   ├── CHANGELOG.md
  │   └── manuals/          # If end-user docs needed
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

**Create `.gitignore`**:
- [ ] Add sensitive file patterns (`.env`, `*.pem`, `*.key`, `credentials*.json`)
- [ ] Add build artifacts (`dist/`, `build/`, `.vite/`)
- [ ] Add OS files (`.DS_Store`, `Thumbs.db`)
- [ ] Add dependencies (`node_modules/`)

**Example `.gitignore`**:
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

**Root `package.json`**:
```json
{
  "name": "project-name",
  "version": "1.0.0",
  "private": true,
  "workspaces": ["app1", "app2"],
  "scripts": {
    "dev": "npm run dev --workspace=app1 & npm run dev --workspace=app2",
    "test": "npm test --workspace=app1 && npm test --workspace=app2",
    "lint": "npm run lint --workspaces",
    "docs:generate": "npm run docs:generate --workspaces"
  },
  "devDependencies": {
    "husky": "^9.0.0"
  }
}
```

**Tasks**:
- [ ] Create root package.json
- [ ] Set up workspaces if monorepo
- [ ] Add parallel scripts for dev (`&`)
- [ ] Add sequential scripts for tests/build (`&&`)
- [ ] Install Husky: `npm install --save-dev husky`
- [ ] Initialize Husky: `npx husky init`

### 5. Git Hooks

**Pre-push hook** (`.husky/pre-push`):
```bash
#!/bin/sh
npm test
```

**Tasks**:
- [ ] Create `.husky/pre-push` hook
- [ ] Make executable: `chmod +x .husky/pre-push`
- [ ] Test: Try `git push` and verify tests run

### 6. Testing Setup

**Each app's `vite.config.js`**:
```javascript
import { defineConfig } from 'vite';
import react from '@vitejs/plugin-react';

export default defineConfig({
  plugins: [react()],
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

**Tasks**:
- [ ] Install: `npm install --save-dev vitest jsdom @testing-library/react @testing-library/jest-dom`
- [ ] Create `src/test-setup.js` in each app
- [ ] Add test script to each app's package.json: `"test": "vitest run"`
- [ ] Add watch script: `"test:watch": "vitest"`
- [ ] Write initial smoke test to verify setup

### 7. Internationalization (if needed)

**Install**:
```bash
npm install i18next react-i18next
```

**Tasks**:
- [ ] Create `src/i18n.js` (see pattern guide)
- [ ] Create `src/locales/en.json` and `src/locales/es.json`
- [ ] Initialize i18n in main entry point
- [ ] Create `LanguageSwitcher` component
- [ ] Test language switching

### 8. Documentation Structure

- [ ] Create `docs/SPEC.md` with technical specification
- [ ] Create `docs/TODO.md` with initial task list
- [ ] Create `docs/CHANGELOG.md` with format:
  ```markdown
  # Changelog

  ## [Unreleased]
  ### Added
  ### Changed
  ### Fixed

  ## [1.0.0] - YYYY-MM-DD
  ### Added
  - Initial release
  ```
- [ ] Create `docs/manuals/` if end-user documentation needed
- [ ] Set up `docs:generate` script if auto-copying docs

### 9. CI/CD Setup

**Create `.github/workflows/test.yml`**:
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

**Tasks**:
- [ ] Create test workflow
- [ ] Create deployment workflow (GitHub Pages, Vercel, etc.)
- [ ] Configure proper `base` paths in vite.config.js for subpath deployment
- [ ] Test CI by pushing to feature branch

### 10. CLAUDE.md Completion

**Fill in all sections**:
- [ ] Project Overview (purpose + tech approach)
- [ ] Commands (with ports and output locations)
- [ ] Architecture (structure + data flow + technical details)
- [ ] Deployment (platform + URL structure + CI/CD)
- [ ] Git Conventions (commits + skills + rules)
- [ ] Security rules (what never to commit)

**Verify**:
- [ ] All npm commands are documented
- [ ] Port numbers are specified
- [ ] Build output locations are clear
- [ ] Git workflow is explained
- [ ] Sensitive file patterns are listed

## Development Phase

### First Feature
- [ ] Create feature branch: `git checkout -b feat/initial-setup`
- [ ] Write feature with tests
- [ ] Run tests: `npm test`
- [ ] Run linter: `npm run lint`
- [ ] Commit with conventional commit: `git ch "feat: add initial feature"`
- [ ] Push and verify CI passes: `git push`

### Custom Skills Setup (if using Claude Code)

**Create `/ship` skill**:
- [ ] Define skill behavior:
  1. Update CHANGELOG.md
  2. Update TODO.md
  3. Run docs:generate if needed
  4. Stage specific files
  5. Commit
  6. Push
- [ ] Document in CLAUDE.md

**Create `/release` skill**:
- [ ] Define skill behavior:
  1. Move CHANGELOG [Unreleased] to [vX.Y.Z]
  2. Update package.json version
  3. Create git tag
  4. Push tag
  5. Create GitHub Release
- [ ] Document in CLAUDE.md

## Security Review

- [ ] Review `.gitignore` for sensitive patterns
- [ ] Verify file staging policy (never `git add -A`)
- [ ] Check that private keys/secrets aren't in code
- [ ] Review localStorage usage (no sensitive data)
- [ ] Test that sensitive files are rejected by git

## Deployment Checklist

### First Deployment
- [ ] Build locally: `npm run build --workspaces`
- [ ] Test built apps locally
- [ ] Configure deployment platform (GitHub Pages, Vercel, etc.)
- [ ] Set up environment variables if needed
- [ ] Deploy
- [ ] Verify deployed apps work
- [ ] Test all routes/subpaths

### Deployment Verification
- [ ] Main app accessible at root path
- [ ] Admin app accessible at subpath (if applicable)
- [ ] Documentation accessible at expected paths
- [ ] i18n works (test language switching)
- [ ] No console errors
- [ ] Mobile responsive

## Ongoing Maintenance

### Before Each Commit
- [ ] Run tests: `npm test`
- [ ] Stage files by name (never `git add -A`)
- [ ] Use conventional commit format
- [ ] Use `git ch` or `git cai` aliases

### Before Each Push
- [ ] Pre-push hook runs tests automatically
- [ ] All tests pass
- [ ] No uncommitted sensitive files

### When Shipping Features
- [ ] Update CHANGELOG.md under [Unreleased]
- [ ] Update TODO.md
- [ ] Update SPEC.md if architecture changed
- [ ] Run docs:generate if manuals changed
- [ ] Stage all related files
- [ ] Commit and push

### When Creating Releases
- [ ] Move CHANGELOG [Unreleased] to [vX.Y.Z]
- [ ] Update version in package.json
- [ ] Create git tag: `git tag vX.Y.Z`
- [ ] Push tag: `git push --tags`
- [ ] Create GitHub Release with CHANGELOG excerpt

## Pattern Reminders

### ✅ DO
- Stage files by name: `git add src/file.js docs/SPEC.md`
- Use conventional commits: `feat:`, `fix:`, `docs:`, etc.
- Track AI vs. human commits with `git cai` vs. `git ch`
- Run tests before pushing
- Update CHANGELOG.md with each feature
- Document commands with ports in CLAUDE.md
- Keep CLAUDE.md up-to-date

### ❌ DON'T
- Never `git add -A` or `git add .`
- Never commit `*.pem`, `*.key`, `.env`, credentials
- Never store private keys in localStorage/sessionStorage
- Never skip pre-push tests
- Never push without updating docs if structure changed
- Never use ambiguous commit messages

## Quick Reference

**Key Files**:
- `CLAUDE.md` — AI guidance + project overview
- `docs/SPEC.md` — Technical specification
- `docs/TODO.md` — Task backlog
- `docs/CHANGELOG.md` — Release history
- `.husky/pre-push` — Test enforcement
- `.github/workflows/` — CI/CD

**Key Commands**:
```bash
npm run dev              # Start dev servers
npm test                 # Run all tests
npm run lint             # Lint all apps
git ch "message"         # Human commit
git cai "message"        # AI commit
```

**Pattern Guide**:
- See `docs/templates/PROJECT-WORKFLOW-PATTERNS.md` for detailed patterns
- See `docs/templates/CLAUDE.md.template` for CLAUDE.md structure

---

## Notes

- This checklist assumes React + Vite stack. Adapt for other frameworks.
- Not all patterns apply to all projects — pick what makes sense.
- The core value is in: CLAUDE.md, file-specific staging, test gates, and AI commit tracking.
- Start simple, add complexity only when needed.
