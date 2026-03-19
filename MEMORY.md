# Playground memory

Project purpose: a React + Vite playground for ArcGIS Basemap Styles Service v2.
Tech stack: React 18+, Calcite components, MapLibre GL JS v5, Vitest/RTL, Node 25, GitHub Pages.
Standard commands: npm run dev, npm test, npm run lint, npm run build, npm run preview.
Expected architecture: component folders, services, templates, config, utils, and data flow between them.
Git/process conventions: Conventional Commits, AI vs human commit helpers, and custom slash workflows.
Documentation rules: keep TODO.md, SPEC.md, and CHANGELOG.md updated after changes.
Security/staging rules: never commit secrets; stage files explicitly.
Coding standards: functional React patterns, accessibility expectations, Calcite usage, and cautious HTML/CSS edits.
Template/export rules: placeholder usage, token handling, parameter parity, attribution requirements, and zoom conversion guidance.
Testing expectations: unit/integration strategy and quality targets.
Known pitfalls and lessons learned: ArcGIS JS SDK token limitation, Leaflet plugin requirement, Calcite/vite gotchas, and prior export bugs to avoid.
Delivery roadmap: MVP/Phase 2/Phase 3 feature breakdown.
Rationale sections: why major technical choices were made.

# AMPA Digital Membership Card System memory

The CLAUDE.md file is the project guide for the AMPA Digital Membership Card System. Key sections:

Project: Cryptographically secure digital membership cards for AMPA Novaschool Almería. QR-coded PNGs verified client-side via EdDSA (Ed25519) — no backend needed.

Architecture — Two-app monorepo:

verification/ — Public React app. Merchants scan QR → app verifies JWT signature + revocation status client-side.
issuer/ — Admin PWA. Generates Ed25519 keypairs, signs JWTs, renders PNG cards, handles CSV batch imports, manages revocations.
Tech stack: React 19, Vite 7, jose library for crypto, i18n (ES/EN), Node.js 25 required.

Key data: JWT payload {v, iss, sub, name, iat, exp, jti}. Revocation by jti or sub in verification/public/revoked.json. Config (public key, branding) in verification/src/config.json.

Deployment: GitHub Pages — verification at /, issuer at /issuer/.

Dev commands: Always run nvm use first (Node 25). Tests via npm test, pre-push hook runs tests automatically.

Git conventions: Conventional commits, git cai for AI commits, /ship and /release custom skills.