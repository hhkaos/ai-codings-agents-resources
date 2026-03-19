# CLAUDE.md — [Project Name]

> [One-sentence description of what this tool does and who it is for.]

Project-level instructions for Claude Code. Read this at the start of every session.

---

## What this project is

[2–4 sentences describing the tool's purpose, who uses it, and what it produces. Be specific about
whether it is read-only, interactive, educational, etc.]

### Layout overview

- [Describe the top-level layout: e.g. gallery fills full window, clicking opens a split panel, etc.]
- [Note any panels, resize handles, or persistent UI state stored in localStorage.]

---

## Behaviour rules (always follow these)

### TODO.md is the source of truth for progress
- After completing any task (or part of a task), **immediately check it off** in TODO.md.
- Do not batch completions — mark done as soon as each atomic step is finished.
- If a task turns out wrong, out of scope, or superseded, remove or annotate it.

### Proactively propose file updates after changes
After any session where we have:
- Fixed one or more errors (lint, build, or runtime)
- Made architectural decisions not already recorded
- Discovered a new gotcha or API behaviour
- Changed how a module works

…ask the user at the end: **"Want me to update CLAUDE.md / AGENTS.md with what we learned?"**

### Ask before destructive actions
Confirm before: deleting files, force-pushing, resetting git state, dropping things from
package.json. A quick "shall I?" costs nothing; an accidental deletion can cost a lot.

### Keep responses concise
- Prefer showing changed code over explaining it at length.
- Do not repeat back the spec or these instructions to confirm you read them.
- Use markdown link syntax for file references: [filename.js](src/filename.js).

---

## Coding style

- **Plain JavaScript (ES modules).** No TypeScript, no JSX, no framework unless the project explicitly uses one.
- **No over-engineering.** Three similar lines of code is better than a premature abstraction.
- **No unsolicited improvements.** If the task is "fix the lint error", fix the lint error — don't also refactor, add comments, or rename things.
- **No defensive code for impossible cases.** Only validate at real system boundaries (user input, external API responses).
- **No docstrings on untouched functions.** Only add JSDoc if you wrote or substantially changed that function.
- **CSS co-located with its module.** Each `src/<module>/` directory owns its own `.css` file.
- **Imports** — use explicit `.js` extensions on local imports. Named exports preferred over default exports.

---

## Constraints

- **[Constraint 1]** e.g. Read-only — no editing of map properties in-app.
- **[Constraint 2]** e.g. No backend — all data from AGOL public REST API.
- **[Constraint 3]** e.g. Public items only — no IdentityManager / OAuth in v1.

---

## Stack

| Concern | Choice |
|---|---|
| Language | Plain JS (ES modules) |
| Bundler | Vite 7.x |
| Mapping SDK | `@arcgis/core` v5 |
| Map web components | `@arcgis/map-components` v5 |
| UI components | `@esri/calcite-components` v5 |
| CSS | `@esri/calcite-components/main.css` + co-located module CSS |
| Dark mode | `class="calcite-mode-dark"` on `<body>` |

---

## Quick commands

```bash
npm run dev      # Vite dev server (HMR) — http://localhost:5173
npm run build    # Production build — run after every set of changes
npm run lint     # ESLint over src/**/*.js
```

---

## File map

```
[project-name]/
├── index.html              # Entry point. <body class="calcite-mode-dark">
├── package.json
├── vite.config.js          # build.target: "es2020" — no other config needed
├── eslint.config.js        # Flat config. Browser globals declared manually.
├── SPEC.md                 # Full product spec — source of truth for requirements
├── CLAUDE.md               # Claude Code instructions (this file)
├── AGENTS.md               # Technical reference: API patterns, gotchas, lessons learned
├── TODO.md                 # Phased checklist — keep updated as work progresses
└── src/
    ├── main.js             # Entry: imports, wiring, top-level init
    ├── state/
    │   └── state.js        # Module-level state — getState() / setState(patch) only
    └── [module]/
        ├── [module].js
        └── [module].css    # Co-located CSS, imported in main.js
```

---

## ArcGIS API patterns (critical — read before touching map code)

### @arcgis/core v5 — imports
Always use explicit `.js` extension:
```js
import WebMap from "@arcgis/core/WebMap.js";
import ArcGISMap from "@arcgis/core/Map.js";
import * as reactiveUtils from "@arcgis/core/core/reactiveUtils.js";
```
`@arcgis/core/buildAssets` does NOT exist in v5 (was a v4 pattern). Do not import it.

### @arcgis/map-components v5
```js
// Register components as side effects — no "dist/" prefix:
import "@arcgis/map-components/components/arcgis-map";
import "@arcgis/map-components/components/arcgis-scene";

// Wait for view ready:
await mapEl.viewOnReady();  // returns Promise<View>

// Access underlying SDK view:
mapEl.view  // → MapView

// Set viewpoint on the element (not on .view):
mapEl.viewpoint = someViewpoint.clone();
```
To verify a component exists: `ls node_modules/@arcgis/map-components/dist/components/ | grep <name>`.

### @esri/calcite-components v5
```js
// Must import CSS:
import "@esri/calcite-components/main.css";

// Dark mode requires class on ancestor — do NOT rely on color-scheme: dark alone:
// <body class="calcite-mode-dark">

// Component side-effect imports:
import "@esri/calcite-components/components/calcite-button";
import "@esri/calcite-components/components/calcite-input-text";
```

### ArcGIS view elements — visibility
**Never use `display: none` on `<arcgis-map>` or `<arcgis-scene>`.** The view collapses to 0×0
and the internal ResizeObserver does not recover it. Use `visibility: hidden/visible` instead —
the element keeps its layout size and the view stays valid.

### Shared Map instance (when using both MapView and SceneView)
Both views can reference the **same `ArcGISMap`/`WebMap` instance**:
- Basemap layers (with `blendMode`, `effect`, `opacity`) are set once, reflected in both views.
- Do not copy or re-apply layer properties between views — they are already shared.
- Keep both `<arcgis-map>` and `<arcgis-scene>` in DOM from the start; toggle visibility with CSS.
- Never dynamically create/destroy SceneView for tab switching — it causes blank-view bugs.

### Viewpoint sync (2D ↔ 3D)
Use the **element's `.viewpoint` setter** (synchronous, instant) — not `view.goTo()` (async, animates).
Apply a scale correction for Web Mercator distortion:
```js
// 2D → 3D
const vp = mapEl.viewpoint.clone();
vp.scale *= Math.cos((vp.targetGeometry.latitude * Math.PI) / 180);
sceneEl.viewpoint = vp;

// 3D → 2D
const vp = sceneEl.viewpoint.clone();
vp.scale /= Math.cos((vp.targetGeometry.latitude * Math.PI) / 180);
mapEl.viewpoint = vp;
```

### arcgis-map / arcgis-scene — custom UI slots
```html
<arcgis-map>
  <div slot="top-right"><!-- any HTML: buttons, links, etc. --></div>
</arcgis-map>
```
Valid slots: `top-left`, `top-right`, `bottom-left`, `bottom-right`,
`top-start`, `top-end`, `bottom-start`, `bottom-end`.

### arcgis-search — outside map elements
Place `arcgis-search` anywhere in the DOM and link it to a view:
```js
// HTML attribute (initial link):
// <arcgis-search reference-element="mapView"></arcgis-search>

// JS — update when active view changes:
navSearchEl.referenceElement = sceneEl;  // or mapEl
```

### calcite-button — appearances
- `"solid"` — filled (default)
- `"outline"` — outlined, transparent background
- `"outline-fill"` — outlined with solid background — **best for buttons over maps**
- `"transparent"` — avoid over maps; poor contrast

### ESLint globals (flat config)
Browser globals must be declared manually in `eslint.config.js`:
```js
globals: {
  document: "readonly",
  window: "readonly",
  console: "readonly",   // not included by default — always add this
  navigator: "readonly",
  AbortController: "readonly",
  Promise: "readonly",
  URL: "readonly",
}
```

---

## What to avoid

- **No `@arcgis/core/buildAssets`** — v4 pattern, does not exist in v5.
- **No Vite plugin for ArcGIS** — SDK loads workers/assets from CDN by default in v5.
- **No `calcite-mode-auto`** — use explicit `calcite-mode-dark` or `calcite-mode-light`.
- **No top-level `await`** at `es2020` build target — wrap in `async function init() {} init();`.
- **No `display: none` on ArcGIS view elements** — use `visibility: hidden/visible`.
- **No dynamic create/destroy of SceneView** — keep both views in the DOM, toggle with CSS.
- **No direct mutation of shared state** — always use `getState()` / `setState(patch)`.
- **No event listener leaks** — if a function that calls `addEventListener` can run multiple times,
  use an `AbortController` and abort before re-registering.
- **No `display: flex` (or other explicit display) without a matching `[hidden]` override** —
  explicit display rules beat the UA stylesheet's `[hidden] { display: none }`. Fix:
  `#element[hidden] { display: none; }`.

---

## Known 2D vs 3D rendering differences

| Feature | MapView (2D) | SceneView (3D) | Notes |
|---|---|---|---|
| `VectorTileLayer.effect` | ✅ Rendered | ❌ Silently ignored | Detect and warn; do not strip |
| `TileLayer.blendMode` | ✅ Rendered | ✅ Rendered | |

Add entries here as new differences are discovered.

---

## ArcGIS REST API (if used)

```
Item metadata:   https://www.arcgis.com/sharing/rest/content/items/{itemId}?f=json
Thumbnail:       https://www.arcgis.com/sharing/rest/content/items/{itemId}/info/{thumbnail}
Search:          https://www.arcgis.com/sharing/rest/search?q=...&f=json
```

All items in this project are **public** (no auth required). If a fetch fails, show a warning — do not silently swallow errors.

---

## GitHub Pages deployment

`vite.config.js` uses `base: "./"` for relative asset paths. The monorepo GitHub Actions workflow
handles building and deploying. `dist/` and `node_modules/` are excluded from version control.
