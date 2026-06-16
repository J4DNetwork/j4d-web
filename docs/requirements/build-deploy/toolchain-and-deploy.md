---
domain: DEPLOY
last_updated: 2026-06-09
status: Draft
---

# Requirements: Build Toolchain & Pages Deploy

## Overview

This cycle replaces j4d.net's runtime Tailwind CDN with a real Tailwind **v4**
build step, deploys via GitHub Actions to GitHub Pages, preserves the custom
domain, and adds social-share meta plus a favicon. The site is a single-page
brochure (`index.html`) with `logo.png` and `CNAME` (contents `j4d.net`) at the
repo root. RS-001 confirmed a version-pinned recipe (Tailwind v4 CSS-first
`@theme`, `@tailwindcss/cli`, automatic source detection, no
`tailwind.config.js`) verified against current docs.

Traceability: all requirements derive from RS-001 findings.md Q2 (build/deploy
recipe) and the "Implications for Design" review note m2 (assets must be copied
into the published artifact). Visual parity and CNAME preservation are the
critical risks.

## Requirements

### REQ-DEPLOY-001: Tailwind v4 compiles a minified stylesheet
The project must use Tailwind **v4** (`tailwindcss` + `@tailwindcss/cli`, both
pinned `^4.3` or the current v4 patch at implementation time) to compile the
utilities used by `index.html` into a single minified `dist/styles.css`. The
build is driven by a `package.json` script (e.g. `build:css`) invoking the v4
CLI with `--minify`.
[Priority: must]
[Traceability: RS-001 Q2 recipe step 1]

### REQ-DEPLOY-002: CSS-first config with @theme brand palette, no JS config
Configuration must be CSS-first: a `src/input.css` containing
`@import "tailwindcss";` followed by an `@theme` block defining the eight brand
shades as `--color-brand-{50,100,200,500,600,700,800,900}` with the values from
RS-001 (`50:#eff6ff 100:#dbeafe 200:#bfdbfe 500:#3b82f6 600:#2563eb
700:#1d4ed8 800:#1e3a5f 900:#0f2440`). There must be **no `tailwind.config.js`**.
[Priority: must]
[Traceability: RS-001 Q2 recipe step 1 `src/input.css`]

### REQ-DEPLOY-003: index.html drops CDN + inline config, links compiled CSS
`index.html` must remove the `cdn.tailwindcss.com` `<script>` and the inline
`tailwind.config` `<script>`, and add `<link rel="stylesheet"
href="dist/styles.css">`. No runtime Tailwind dependency may remain.
[Priority: must]
[Traceability: RS-001 Q2 recipe; current index.html lines 9-29]

### REQ-DEPLOY-004: Visual parity with the current site
The site rendered from the compiled `dist/styles.css` must look the same as the
current CDN-rendered site. "Visual parity" is verified against the concrete
brand utility-class inventory recorded in RS-001 findings — the rendered site
must resolve **all** of these enumerated brand classes plus all core
utilities to equivalent styling: `bg-brand-{900,600,700,100,50}`,
`text-brand-{900,200,600,700}`, `hover:bg-brand-{100,700}`, and
`hover:text-brand-200`. Parity is checked against this enumerated class
inventory, not subjectively.
[Priority: must]
[Traceability: RS-001 Q2 "classes/config in use" inventory]

### REQ-DEPLOY-005: GitHub Actions workflow builds and deploys to Pages
A workflow at `.github/workflows/deploy.yml` must build the CSS and deploy to
Pages using `actions/upload-pages-artifact@v3` and `actions/deploy-pages@v4`. It
must declare `permissions: contents: read, pages: write, id-token: write`, use
the `github-pages` environment for the deploy job, and set
`concurrency.group: pages` and `concurrency.cancel-in-progress: false` (both
keys of the same concurrency block in the RS-001 recipe). It must trigger on
push to `main` and support `workflow_dispatch`.
[Priority: must]
[Traceability: RS-001 Q2 recipe step 2]

### REQ-DEPLOY-006: CNAME preserved in the published artifact
The workflow must include an explicit step that copies the repo-root `CNAME`
(contents `j4d.net`) into the published artifact directory, so the custom domain
survives the Pages source switch from branch to GitHub Actions (which otherwise
ignores the repo-root CNAME and publishes only the artifact).
[Priority: must]
[Traceability: RS-001 Q2 recipe step 3 "CNAME preservation"]

### REQ-DEPLOY-007: All static assets copied into the published artifact
The workflow's assembly step must copy every static asset the published site
needs into the artifact directory — at minimum `index.html`, `logo.png`, the
compiled `dist/styles.css`, and any newly introduced asset (favicon, OG image).
Any asset not copied will be missing from the published site.
[Priority: must]
[Traceability: RS-001 Implications review-note m2]

### REQ-DEPLOY-008: Open Graph and Twitter Card meta tags
`index.html` must include Open Graph and Twitter Card meta tags: at minimum
`og:title`, `og:description`, `og:url` = `https://j4d.net`, `og:image`, and
`twitter:card`. Values must align with the existing title/description.
[Priority: must]
[Traceability: kickoff.md scope item; RS-001 Implications "OG/Twitter meta"]

### REQ-DEPLOY-009: Favicon
`index.html` must include a `<link rel="icon">` referencing a favicon, reusing
`logo.png` unless a dedicated icon asset is introduced. If a dedicated icon is
introduced, it must also satisfy REQ-DEPLOY-007 (copied into the artifact).
[Priority: must]
[Traceability: kickoff.md scope item]

### REQ-DEPLOY-010: Committed lockfile for reproducible CI install
A `package-lock.json` must be committed so the workflow's `npm ci` install is
reproducible. (Generated once locally via `npm install` during implementation.)
[Priority: should]
[Traceability: RS-001 Q2 notes; Open Questions "package-lock.json"]

### REQ-DEPLOY-011: Manual Pages-source switch (human-operator step)
A one-time manual step must be documented as an acceptance note, not automated:
in repo Settings → Pages → Build and deployment → Source, switch from "Deploy
from a branch" to "GitHub Actions"; then after the first deploy verify the
custom domain `j4d.net` still shows under Settings → Pages (re-enter once if
blank — it then persists because the artifact carries CNAME). This is a
human-operator step and is not workflow-scriptable.
[Priority: must]
[Traceability: RS-001 Q2 recipe step 4 "one-time manual repo setting"]
