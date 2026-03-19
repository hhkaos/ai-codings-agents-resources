# Reusable Project Workflow Patterns

This guide captures successful patterns from the Digital Membership Card System that can be applied to other projects.

## Table of Contents

1. [Architecture Patterns](#architecture-patterns)
2. [Documentation Strategy](#documentation-strategy)
3. [Git Workflow](#git-workflow)
4. [Development Setup](#development-setup)
5. [CI/CD Patterns](#cicd-patterns)
6. [Security Best Practices](#security-best-practices)
7. [Testing Strategy](#testing-strategy)
8. [Internationalization](#internationalization)

---

## Architecture Patterns

### Two-App Monorepo for Client-Side Systems

**When to use**: Projects with a public-facing app and an admin/management tool.

**Structure**:
```
project-root/
├── app1/                 # Public app
│   ├── src/
│   ├── public/
│   ├── package.json
│   └── vite.config.js
├── app2/                 # Admin app
│   ├── src/
│   ├── public/
│   ├── package.json
│   └── vite.config.js
├── docs/                 # Shared documentation
├── package.json          # Root package.json with workspace scripts
└── CLAUDE.md            # AI guidance
```

**Benefits**:
- Clear separation of concerns
- Independent deployment
- Shared documentation
- Root-level scripts for convenience

**Root package.json pattern**:
```json
{
  "scripts": {
    "dev": "npm run dev --workspace=app1 & npm run dev --workspace=app2",
    "test": "npm test --workspace=app1 && npm test --workspace=app2",
    "install:all": "npm install && npm install --workspace=app1 && npm install --workspace=app2"
  }
}
```

### Client-Side Cryptographic Verification

**Pattern**: No-backend architecture using cryptographic signatures.

**Components**:
1. **Key Generation**: Admin app generates keypair in browser
2. **Signing**: Admin app signs data client-side
3. **Distribution**: Signed artifacts distributed as static files or QR codes
4. **Verification**: Public app verifies signatures with embedded public key

**Libraries**:
- `jose` for EdDSA (Ed25519)
- `qrcode` for QR generation
- `jszip` for batch exports

**Config Pattern** (`verification/src/config.json`):
```json
{
  "publicKey": "PUBLIC_KEY_HERE",
  "issuer": "Organization Name",
  "branding": { "primaryColor": "#color", "logo": "/path" },
  "revocation": { "checkUrl": "/revoked.json" }
}
```

### Static Revocation System

**Pattern**: Client-side revocation checking without backend.

**File structure** (`public/revoked.json`):
```json
{
  "lastUpdated": "2024-01-15T10:00:00Z",
  "revokedJTIs": ["jti1", "jti2"],
  "revokedSubjects": ["sub1"]
}
```

**Two-level revocation**:
- `jti` (token ID) — Revokes single issuance
- `sub` (subject) — Revokes all tokens for a member

**Update workflow**:
1. Admin generates revocation update in issuer app
2. Admin manually updates `verification/public/revoked.json`
3. Commit + push (auto-deploys via CI)

---

## Documentation Strategy

### CLAUDE.md as Single Source of Truth

**Structure**:
```markdown
# CLAUDE.md
## Project Overview (elevator pitch + tech)
## Commands (all commands, organized by category)
## Architecture (structure + data flow + technical details)
## Git Conventions (commits + custom workflows + rules)
```

**Benefits**:
- AI can understand project instantly
- New developers have complete reference
- Reduces repetitive questions
- Forces clarity in design decisions

### Multi-Tier Documentation

**Tier 1: Developer Docs** (root `/docs/`)
- `SPEC.md` — Technical specification
- `TODO.md` — Task backlog
- `CHANGELOG.md` — Release history
- `PLAN.md` — Current work plan

**Tier 2: End-User Manuals** (`docs/manuals/end-user/`)
```
docs/manuals/end-user/
├── issuer/
│   ├── es/
│   │   └── README.md       # Spanish manual for issuer
│   └── en/
│       └── README.md       # English manual for issuer
└── verifier/
    ├── es/
    │   └── README.md       # Spanish manual for verifier
    └── en/
        └── README.md       # English manual for verifier
```

**Tier 3: Auto-Generated Public Docs**
- Script copies manuals to app `public/` folders
- Accessible at runtime via Help buttons
- Command: `npm run docs:generate`

**Documentation Update Rule**:
```
IF docs/manuals/** changes THEN:
  1. Run npm run docs:generate
  2. Stage generated public/docs/** files
  3. Commit together
```

### docs:generate Pattern

**Root package.json**:
```json
{
  "scripts": {
    "docs:generate": "npm run docs:generate --workspace=app1 && npm run docs:generate --workspace=app2"
  }
}
```

**App-level script** (e.g., `verification/package.json`):
```javascript
// Copies docs/manuals/end-user/verifier/** → verification/public/docs/
// Preserves language structure
```

---

## Git Workflow

### Conventional Commits with AI Tracking

**Aliases** (`.gitconfig` or global):
```bash
[alias]
    ch = "!f() { git commit -m \"$1\"; }; f"
    cai = "!f() { git commit -m \"AI: $1\" --author=\"AI Assistant <ai@example.com>\"; }; f"
```

**Usage**:
```bash
git ch "feat: add user authentication"     # Human commit
git cai "fix: resolve token validation bug" # AI commit
```

**Benefits**:
- Clear attribution in git history
- Easier to filter/review AI changes
- Helps identify areas needing human review

### Custom Git Skills

**`/ship` Skill** — Commit + Push + Documentation Update
```
1. Update CHANGELOG.md (under [Unreleased])
2. Update TODO.md (mark completed tasks)
3. Update SPEC.md (if architecture changed)
4. IF docs/manuals changed → Run npm run docs:generate
5. Stage specific files (never git add -A)
6. Commit with conventional commit message
7. Push to remote
```

**`/release` Skill** — Versioned Release
```
1. Move [Unreleased] section in CHANGELOG.md to [vX.Y.Z]
2. Update version in package.json
3. Create git tag
4. Push tag
5. Create GitHub Release with CHANGELOG excerpt
```

### File Staging Policy

**Rule**: Always stage files by name.

**Why**: Prevents accidentally committing:
- Sensitive files (`.env`, `.pem`, private keys)
- Build artifacts
- OS-specific files (`.DS_Store`)
- IDE configurations

**Pattern**:
```bash
# ❌ NEVER
git add -A
git add .

# ✅ ALWAYS
git add src/component.js src/component.test.js
git add docs/CHANGELOG.md docs/TODO.md
```

### Pre-Push Hook

**Setup** (`package.json`):
```json
{
  "scripts": {
    "test": "npm test --workspace=app1 && npm test --workspace=app2"
  },
  "devDependencies": {
    "husky": "^9.0.0"
  }
}
```

**Hook** (`.husky/pre-push`):
```bash
#!/bin/sh
npm test
```

**Benefit**: Ensures tests pass before pushing to remote.

---

## Development Setup

### Monorepo Script Patterns

**Root `package.json`**:
```json
{
  "scripts": {
    "dev": "npm run dev --workspace=app1 & npm run dev --workspace=app2",
    "test": "npm test --workspace=app1 && npm test --workspace=app2",
    "lint": "npm run lint --workspace=app1 && npm run lint --workspace=app2",
    "docs:generate": "npm run docs:generate --workspaces"
  },
  "workspaces": ["app1", "app2"]
}
```

**Key patterns**:
- `&` for parallel execution (dev servers)
- `&&` for sequential execution (tests that should fail-fast)
- `--workspace` for targeting specific apps
- `--workspaces` for running across all

### Port Conventions

**Pattern**: Sequential ports for related apps.

Example:
- Verification (public): `:5173`
- Issuer (admin): `:5174`

**Document in CLAUDE.md**:
```markdown
## Commands

```bash
npm run dev                          # verification :5173, issuer :5174
```
```

---

## CI/CD Patterns

### GitHub Pages Deployment with Subpaths

**Challenge**: Deploy multiple apps from monorepo to single GitHub Pages site.

**Solution**:

**App1** (root path `/`):
```javascript
// vite.config.js
export default {
  base: '/'
}
```

**App2** (subpath `/app2/`):
```javascript
// vite.config.js
export default {
  base: '/app2/'
}
```

**GitHub Actions Workflow**:
```yaml
name: Deploy to Pages

on:
  push:
    branches: [main]

jobs:
  build-deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '20'

      - name: Install dependencies
        run: npm install && npm install --workspaces

      - name: Run tests
        run: npm test

      - name: Build app1
        run: npm run build --workspace=app1

      - name: Build app2
        run: npm run build --workspace=app2

      - name: Combine artifacts
        run: |
          mkdir -p dist
          cp -r app1/dist/* dist/
          mkdir -p dist/app2
          cp -r app2/dist/* dist/app2/

      - name: Deploy to Pages
        uses: actions/upload-pages-artifact@v2
        with:
          path: dist
```

**Testing CI** (separate workflow):
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

---

## Security Best Practices

### Never Commit Sensitive Files

**Patterns to block**:
- `*.pem` — Private keys
- `*.key` — Key files
- `.env` — Environment variables
- `credentials.json` — Credentials
- `test-data/` with real user data

**`.gitignore`**:
```
# Security
*.pem
*.key
.env
.env.local
credentials*.json

# Test data
test-data/real/
```

**Enforcement**: Use file-specific staging (never `git add -A`).

### Client-Side Key Handling

**Pattern**: Keys exist only in browser memory, never persisted.

**Implementation**:
```javascript
// ❌ DON'T
localStorage.setItem('privateKey', key);
sessionStorage.setItem('privateKey', key);

// ✅ DO
let privateKey = null;  // Module-scoped variable

function generateKey() {
  privateKey = await generateKeyPair();
  // Use immediately, then clear on page unload
}

window.addEventListener('beforeunload', () => {
  privateKey = null;
});
```

**Export Pattern**:
```javascript
// Export as download, never store
function exportPrivateKey() {
  const blob = new Blob([privateKey], { type: 'application/x-pem-file' });
  downloadBlob(blob, 'private-key.pem');

  // Show warning
  alert('Store this file securely. It will not be saved by the application.');
}
```

---

## Testing Strategy

### Vitest + Testing Library Pattern

**Setup** (`vite.config.js`):
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

**Test Organization**:
```
src/
├── components/
│   ├── Button.jsx
│   └── Button.test.jsx
├── utils/
│   ├── crypto.js
│   └── crypto.test.js
└── test-setup.js
```

**Test Patterns**:

**Component tests**:
```javascript
import { render, screen } from '@testing-library/react';
import { describe, it, expect } from 'vitest';
import Button from './Button';

describe('Button', () => {
  it('renders with text', () => {
    render(<Button>Click me</Button>);
    expect(screen.getByText('Click me')).toBeInTheDocument();
  });
});
```

**Crypto/Logic tests**:
```javascript
import { describe, it, expect } from 'vitest';
import { generateKeyPair, signToken, verifyToken } from './crypto';

describe('Crypto operations', () => {
  it('generates valid keypair', async () => {
    const { publicKey, privateKey } = await generateKeyPair();
    expect(publicKey).toBeTruthy();
    expect(privateKey).toBeTruthy();
  });
});
```

---

## Internationalization

### i18n Pattern with Language Switcher

**Structure**:
```
src/
├── i18n.js              # Configuration
├── locales/
│   ├── en.json          # English translations
│   └── es.json          # Spanish translations
└── components/
    └── LanguageSwitcher.jsx
```

**i18n Setup** (`src/i18n.js`):
```javascript
import i18n from 'i18next';
import { initReactI18next } from 'react-i18next';
import en from './locales/en.json';
import es from './locales/es.json';

i18n
  .use(initReactI18next)
  .init({
    resources: {
      en: { translation: en },
      es: { translation: es }
    },
    lng: localStorage.getItem('language') || 'es',
    fallbackLng: 'en',
    interpolation: {
      escapeValue: false
    }
  });

export default i18n;
```

**Translation File Pattern** (`locales/en.json`):
```json
{
  "common": {
    "yes": "Yes",
    "no": "No",
    "cancel": "Cancel"
  },
  "nav": {
    "home": "Home",
    "about": "About"
  },
  "errors": {
    "notFound": "Not found",
    "serverError": "Server error"
  }
}
```

**Usage in Components**:
```javascript
import { useTranslation } from 'react-i18next';

function MyComponent() {
  const { t } = useTranslation();

  return (
    <div>
      <h1>{t('nav.home')}</h1>
      <button>{t('common.yes')}</button>
    </div>
  );
}
```

**Language Switcher**:
```javascript
import { useTranslation } from 'react-i18next';

function LanguageSwitcher() {
  const { i18n } = useTranslation();

  const changeLanguage = (lng) => {
    i18n.changeLanguage(lng);
    localStorage.setItem('language', lng);
  };

  return (
    <select value={i18n.language} onChange={(e) => changeLanguage(e.target.value)}>
      <option value="en">English</option>
      <option value="es">Español</option>
    </select>
  );
}
```

---

## Summary Checklist

When starting a new project based on these patterns:

- [ ] Create CLAUDE.md with all sections filled
- [ ] Set up monorepo structure if multiple apps needed
- [ ] Configure git aliases for conventional commits + AI tracking
- [ ] Add Husky pre-push hook for tests
- [ ] Set up GitHub Actions for tests + deployment
- [ ] Create `.gitignore` for sensitive files
- [ ] Implement file-specific staging policy
- [ ] Configure i18n from start (easier than retrofitting)
- [ ] Set up Vitest with proper config
- [ ] Create docs/ structure with CHANGELOG, TODO, SPEC
- [ ] Add custom /ship and /release skills if using Claude Code
- [ ] Document all commands in CLAUDE.md with ports
- [ ] Set up docs:generate script if using manual docs
- [ ] Configure proper base paths for deployment

---

## Evolution Notes

**Key insights from this project**:

1. **CLAUDE.md is transformative** — Single best investment for AI-assisted development
2. **File-specific staging saves headaches** — Prevented multiple near-misses with sensitive data
3. **AI commit tracking is valuable** — Makes code review much clearer
4. **Client-side crypto is viable** — No backend needed for many use cases
5. **Documentation automation pays off** — Auto-copying manuals to public/ eliminated sync issues
6. **Monorepo scripts need care** — Parallel (`&`) vs. sequential (`&&`) matters
7. **Test gates are essential** — Pre-push hook caught issues many times
8. **i18n from day 1** — Much easier than adding later
9. **Custom skills improve velocity** — /ship reduced deployment steps from 8 to 1

**Anti-patterns avoided**:
- ❌ No `git add -A` — Would have committed private keys
- ❌ No localStorage for private keys — Security risk
- ❌ No manual doc syncing — Would have gotten out of sync
- ❌ No versioned dependencies without ranges — Kept up-to-date automatically
- ❌ No tests without CI enforcement — Pre-push hook ensures quality
