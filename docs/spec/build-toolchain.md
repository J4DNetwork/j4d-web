---
status: Approved
last_updated: 2026-06-09
requires: [REQ-DEPLOY-001, REQ-DEPLOY-002, REQ-DEPLOY-003, REQ-DEPLOY-004, REQ-DEPLOY-010]
---

# Build Toolchain — Tailwind v4

## Context

The site currently loads Tailwind from `cdn.tailwindcss.com` at runtime with an
inline `tailwind.config` `<script>` (`index.html` lines 9-29). This spec defines
the replacement: a real Tailwind **v4** build step that compiles the utilities
used by `index.html` into a single minified `dist/styles.css`, with CSS-first
configuration (no `tailwind.config.js`) and a committed lockfile for reproducible
CI installs. The recipe and all values are pinned by RS-001 Q2 (verified against
current Tailwind v4 docs, latest stable v4.3.0). Fulfills REQ-DEPLOY-001..004 and
010.

## Design

### D1. package.json contract (REQ-DEPLOY-001, 010)

`package.json` at the repo root declares Tailwind v4 as dev dependencies and a
`build:css` script driving the v4 CLI with `--minify`:

```json
{
  "name": "j4d-web",
  "private": true,
  "devDependencies": {
    "tailwindcss": "^4.3.0",
    "@tailwindcss/cli": "^4.3.0"
  },
  "scripts": {
    "build:css": "tailwindcss -i ./src/input.css -o ./dist/styles.css --minify"
  }
}
```

Decisions / rationale:

- **Two packages, same minor:** v4 split the CLI into `@tailwindcss/cli` (which
  provides the `tailwindcss` binary) atop the `tailwindcss` engine. Both are
  installed as devDeps and pinned to the same `^4.3.0` minor. Confirm the current
  v4 patch at implementation time; do not regress below 4.3.
- **`build:css` script** resolves the `tailwindcss` bin from `@tailwindcss/cli`;
  equivalent to `npx @tailwindcss/cli -i ./src/input.css -o ./dist/styles.css
  --minify`.
- **Committed `package-lock.json` (REQ-DEPLOY-010):** generated once locally via
  `npm install` and committed so the workflow's `npm ci` is reproducible. The
  lockfile must pin the resolved v4 patch versions.
- **`dist/styles.css` is a build artifact** and need not be committed; CI builds
  it (see deploy-pipeline spec).

### D2. src/input.css — CSS-first config (REQ-DEPLOY-002)

`src/input.css` is the entire Tailwind configuration. There is **no
`tailwind.config.js`** anywhere in the project.

```css
@import "tailwindcss";

@theme {
  --color-brand-50:  #eff6ff;
  --color-brand-100: #dbeafe;
  --color-brand-200: #bfdbfe;
  --color-brand-500: #3b82f6;
  --color-brand-600: #2563eb;
  --color-brand-700: #1d4ed8;
  --color-brand-800: #1e3a5f;
  --color-brand-900: #0f2440;
}
```

Decisions / rationale:

- **`@theme` generates utilities:** v4 generates `bg-brand-600`, `text-brand-900`,
  `hover:bg-brand-700`, etc. directly from the `--color-brand-*` custom
  properties. No `content`/`extend` block.
- **Values are migrated verbatim** from the current inline config (lines 14-26),
  so the palette is identical to today's CDN rendering.
- **No `@source` directive:** v4 auto-detects sources (scans non-ignored,
  non-binary, non-CSS, non-lockfile files). For a single root `index.html` built
  via the CLI, `index.html` is detected automatically. `@source` would only be
  needed to scan `node_modules`, change a base path, exclude legacy dirs, or
  safelist — none apply here. [If a future build moves `index.html` out of the
  CLI's detected root, add `@source "./index.html";` — fallback only.]

### D3. index.html head changes (REQ-DEPLOY-003)

In `index.html` `<head>`:

- **Remove** the `<script src="https://cdn.tailwindcss.com"></script>` (line 9).
- **Remove** the inline `<script> tailwind.config = { … } </script>` block
  (lines 10-29).
- **Add** `<link rel="stylesheet" href="dist/styles.css">`.

No runtime Tailwind dependency may remain after this change. (OG/Twitter meta and
favicon `<link>` additions to the same `<head>` are specified separately in
deploy-pipeline.md.)

### D4. Visual parity (REQ-DEPLOY-004)

Parity is verified against the enumerated brand-class inventory from RS-001, not
subjectively. The compiled `dist/styles.css` must resolve **all** of these brand
classes to styling equivalent to the current CDN render, plus all core utilities:

- Backgrounds: `bg-brand-900`, `bg-brand-600`, `bg-brand-700`, `bg-brand-100`,
  `bg-brand-50`
- Text: `text-brand-900`, `text-brand-200`, `text-brand-600`, `text-brand-700`
- Borders: `border-brand-800`
- Hover backgrounds: `hover:bg-brand-100`, `hover:bg-brand-700`
- Hover text: `hover:text-brand-200`, `hover:text-brand-900`
- Core utilities present in `index.html`: layout (`max-w-*`, `mx-auto`, `px/py-*`,
  `flex`, `grid md:grid-cols-3`, `gap-*`), typography (`text-*`, `font-*`,
  `leading-*`, `tracking-tight`, `antialiased`), borders/radius
  (`border`, `rounded-*`), effects (`hover:shadow-lg`, `transition`), and the
  `sm:`/`md:` responsive variants used.

The eight `--color-brand-*` shades must be present in `@theme` so every brand
utility above is generated. No plugins are required. Note that
`border-brand-800` is the **only** consumer of brand shade 800; enumerating it
here (and in `build_visual_parity_classes` below) ensures shade 800's parity is
actually asserted by the test rather than merely relying on it being
auto-generated from `@theme`.

## Verification

### Automated

- **build_css_compiles**: `npm ci && npm run build:css` exits 0 and produces a
  non-empty minified `dist/styles.css`. [REQ-DEPLOY-001]
- **build_no_js_config**: no `tailwind.config.js`/`.cjs`/`.mjs` exists in the
  repo. [REQ-DEPLOY-002]
- **build_input_css_theme**: `src/input.css` contains `@import "tailwindcss";`
  and an `@theme` block defining all eight `--color-brand-{50,100,200,500,600,
  700,800,900}` with the pinned values. [REQ-DEPLOY-002]
- **build_html_no_cdn**: `index.html` contains no `cdn.tailwindcss.com` and no
  inline `tailwind.config`; it contains `<link rel="stylesheet"
  href="dist/styles.css">`. [REQ-DEPLOY-003]
- **build_visual_parity_classes**: the compiled `dist/styles.css` contains rules
  for every enumerated brand class in D4 — grep for each of `.bg-brand-900`,
  `.bg-brand-600`, `.bg-brand-700`, `.bg-brand-100`, `.bg-brand-50`,
  `.text-brand-900`, `.text-brand-200`, `.text-brand-600`, `.text-brand-700`,
  `.border-brand-800`, `.hover\:bg-brand-100:hover`, `.hover\:bg-brand-700:hover`,
  `.hover\:text-brand-200:hover`, `.hover\:text-brand-900:hover`.
  `.border-brand-800` (shade 800's only consumer) and `.hover\:text-brand-900`
  must be asserted explicitly. [REQ-DEPLOY-004]
- **build_lockfile_present**: `package-lock.json` exists and pins resolved
  `tailwindcss`/`@tailwindcss/cli` v4 versions. [REQ-DEPLOY-010]

### Manual

- Open the site rendered against the compiled `dist/styles.css` and visually
  compare to the current CDN render — nav/hero/contact brand-900 backgrounds,
  brand-600 buttons, brand-200 hover text, brand-50/600 service icons, and
  brand-100/700 pain-point badges all match. [REQ-DEPLOY-004]

### Acceptance Criteria

- [ ] `package.json` declares `tailwindcss`+`@tailwindcss/cli` `^4.3.0` and a
      `build:css` script using `--minify`. [REQ-DEPLOY-001]
- [ ] `src/input.css` is CSS-first with `@import "tailwindcss";` + `@theme`
      brand palette; no `tailwind.config.js`. [REQ-DEPLOY-002]
- [ ] `index.html` drops the CDN script + inline config and links
      `dist/styles.css`; no runtime Tailwind remains. [REQ-DEPLOY-003]
- [ ] All enumerated brand classes resolve in `dist/styles.css`; visual parity
      holds. [REQ-DEPLOY-004]
- [ ] `package-lock.json` committed; `npm ci` reproducible. [REQ-DEPLOY-010]
- [ ] `npm run build:css` runs clean (no errors/warnings).
