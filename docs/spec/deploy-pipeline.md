---
status: Approved
last_updated: 2026-06-09
requires: [REQ-DEPLOY-005, REQ-DEPLOY-006, REQ-DEPLOY-007, REQ-DEPLOY-008, REQ-DEPLOY-009, REQ-DEPLOY-011]
---

# Deploy Pipeline — GitHub Actions → Pages

## Context

After switching off the Tailwind CDN to a compiled stylesheet (build-toolchain
spec), the site must be built and published by CI rather than served directly
from a branch. This spec defines the GitHub Actions Pages workflow, the
mandatory CNAME preservation step (the critical custom-domain risk), the
static-asset assembly step, the Open Graph / Twitter Card meta and favicon
additions to `index.html`, and the one-time manual human-operator step to flip
the Pages source. All action versions and the recipe shape are pinned by RS-001
Q2. Fulfills REQ-DEPLOY-005..009 and 011.

## Design

### D1. Workflow contract (REQ-DEPLOY-005)

A workflow at `.github/workflows/deploy.yml` with two jobs:

- **Triggers:** `push` to `main`, and `workflow_dispatch`.
- **Permissions (top-level):** `contents: read`, `pages: write`,
  `id-token: write`.
- **Concurrency:** `group: pages`, `cancel-in-progress: false` (both keys of one
  concurrency block — a deploy in flight must not be cancelled).
- **build job** (`ubuntu-latest`):
  1. `actions/checkout@v4`
  2. `actions/setup-node@v4` with `node-version: 20`
  3. `npm ci`
  4. `npm run build:css` (produces `dist/styles.css`, minified)
  5. Assemble `_site/` (see D2)
  6. `actions/upload-pages-artifact@v3` with `path: ./_site`
- **deploy job** (`needs: build`, `ubuntu-latest`):
  - `environment: { name: github-pages, url: ${{ steps.deployment.outputs.page_url }} }`
  - step `id: deployment` uses `actions/deploy-pages@v4`

Pinned versions (do not regress): `checkout@v4`, `setup-node@v4`,
`upload-pages-artifact@v3`, `deploy-pages@v4`, Node 20. Rationale:
`deploy-pages` must be v4+ to consume `upload-pages-artifact@v3` (post-Dec-2024
artifacts-v4 deprecation); separate build/deploy jobs with the `github-pages`
environment is the current GitHub-recommended shape.

### D2. Site assembly and asset copy (REQ-DEPLOY-007)

The build job's "Assemble site" step copies **every** static asset the published
site needs into `_site/`. At minimum:

```bash
mkdir -p _site/dist
cp index.html logo.png _site/
cp dist/styles.css _site/dist/styles.css
cp CNAME _site/CNAME            # CNAME preservation — see D3
```

Contract: any asset referenced by the published `index.html` (the compiled
stylesheet, `logo.png` used as both nav logo and OG/favicon image) MUST appear in
`_site/`. If a dedicated favicon or OG image asset is introduced later, the copy
step MUST be extended to include it (REQ-DEPLOY-007/009) — otherwise it 404s on
the live site. The flat layout mirrors current relative paths
(`dist/styles.css`, `logo.png`); if the publish layout changes, update both the
`cp` steps and the `<link>`/`<img>` hrefs.

### D3. CNAME preservation (REQ-DEPLOY-006)

`cp CNAME _site/CNAME` is **mandatory**. **Failure mode:** when the Pages source
is switched from "deploy from a branch" to "GitHub Actions", GitHub ignores any
repo-root `CNAME` and publishes *exactly* the uploaded artifact. If `CNAME`
(contents `j4d.net`) is absent from `_site/`, the custom domain `j4d.net` breaks
(reverts to `*.github.io` and the Pages custom-domain setting goes blank). With
`CNAME` in the artifact, `j4d.net` keeps resolving and the setting persists. This
step is a hard acceptance gate, not optional cleanup.

### D4. Open Graph / Twitter Card meta (REQ-DEPLOY-008)

Add the following to `index.html` `<head>`, with values aligned to the existing
`<title>` and `meta description`:

```html
<meta property="og:type" content="website">
<meta property="og:title" content="J4D Network — AI Consulting for K-12 Education">
<meta property="og:description" content="We help K-12 schools and districts integrate AI into teacher workflows — practically, safely, and on their terms.">
<meta property="og:url" content="https://j4d.net">
<meta property="og:image" content="https://j4d.net/logo.png">
<meta name="twitter:card" content="summary">
<meta name="twitter:title" content="J4D Network — AI Consulting for K-12 Education">
<meta name="twitter:description" content="We help K-12 schools and districts integrate AI into teacher workflows — practically, safely, and on their terms.">
<meta name="twitter:image" content="https://j4d.net/logo.png">
```

Decisions / rationale:

- **`og:url` = `https://j4d.net`** (matches the existing canonical link).
- **`og:image`/`twitter:image` default to `logo.png`** (absolute URL) since no
  dedicated share image exists in source (Open Questions default). If a dedicated
  OG image is later introduced, swap the href and add it to the D2 copy step.
- **`twitter:card = summary`** (not `summary_large_image`) because the asset is a
  square logo, not a wide banner; `summary_large_image` is acceptable only if a
  wide dedicated image is added.
- **Values mirror title/description** verbatim to keep share previews consistent
  with the page.

### D5. Favicon (REQ-DEPLOY-009)

Add to `index.html` `<head>`:

```html
<link rel="icon" href="logo.png">
```

Default reuses `logo.png` (already copied into `_site/` per D2). If a dedicated
icon asset (`.ico`/`.png`) is later introduced, update both this `<link>` and the
D2 copy step (REQ-DEPLOY-007).

### D6. Manual human-operator step (REQ-DEPLOY-011)

This step is **documented, not CI-tested** — it is UI-only and not
workflow-scriptable, so it is NOT a CI acceptance criterion:

1. Repo **Settings → Pages → Build and deployment → Source** → switch from
   "Deploy from a branch" to **"GitHub Actions"**.
2. After the first successful deploy, verify **Settings → Pages** still shows the
   custom domain **`j4d.net`**. If blank, re-enter it once — it then persists
   because the artifact carries `CNAME` (D3).

This is recorded as an acceptance note for the operator, executed once.

## Verification

### Automated

- **deploy_workflow_shape**: `.github/workflows/deploy.yml` triggers on push to
  `main` + `workflow_dispatch`; declares `permissions` `contents:read`,
  `pages:write`, `id-token:write`; sets `concurrency.group: pages` and
  `cancel-in-progress: false`; build job runs checkout@v4, setup-node@v4 (node
  20), `npm ci`, `npm run build:css`; uses `upload-pages-artifact@v3`; deploy job
  `needs: build`, `environment: github-pages`, `deploy-pages@v4`.
  [REQ-DEPLOY-005]
- **deploy_assembles_assets**: the assemble step copies `index.html`, `logo.png`,
  `dist/styles.css`, and `CNAME` into `_site/` (plus any added favicon/OG asset).
  [REQ-DEPLOY-007]
- **deploy_cname_preserved**: the workflow contains `cp CNAME _site/CNAME` (or
  equivalent) so `CNAME` lands in the artifact root. [REQ-DEPLOY-006]
- **deploy_og_twitter_meta**: `index.html` head contains `og:title`,
  `og:description`, `og:url`=`https://j4d.net`, `og:image`, `og:type`=`website`,
  and `twitter:card`/`title`/`description`/`image`. [REQ-DEPLOY-008]
- **deploy_favicon**: `index.html` head contains `<link rel="icon" …>`.
  [REQ-DEPLOY-009]

### Manual

- **deploy_pages_source_switch**: perform the D6 one-time Pages-source switch and
  confirm `j4d.net` persists after the first deploy. [REQ-DEPLOY-011] (Manual by
  definition — not a CI-testable criterion.)
- **deploy_live_smoke**: after deploy, load `https://j4d.net`, confirm the styled
  page renders (no CDN), `logo.png`/favicon load, and a link-share preview shows
  the OG title/description/image.

### Acceptance Criteria

- [ ] `.github/workflows/deploy.yml` matches the D1 contract (triggers,
      permissions, concurrency, pinned action versions, two-job shape).
      [REQ-DEPLOY-005]
- [ ] Assemble step copies `index.html`, `logo.png`, `dist/styles.css`, `CNAME`
      (and any new asset) into `_site/`. [REQ-DEPLOY-007]
- [ ] `cp CNAME _site/CNAME` present; failure mode documented. [REQ-DEPLOY-006]
- [ ] OG + Twitter meta present with aligned values; `og:url` = `https://j4d.net`.
      [REQ-DEPLOY-008]
- [ ] Favicon `<link rel="icon">` present (defaults to `logo.png`).
      [REQ-DEPLOY-009]
- [ ] Manual Pages-source switch documented as a non-CI operator step.
      [REQ-DEPLOY-011]
