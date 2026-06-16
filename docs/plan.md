# Implementation Plan: j4d.net Integrity & Toolchain Cycle

## Status

In progress (sdd-implement, 2026-06-09). Chunks 0–4 complete (local verify
green); Chunk 5 BLOCKED: human-operator (GitHub UI Pages-source switch — not
scriptable); Chunk 6 static portion complete, live-smoke DEFERRED to operator
(depends on Chunk 5 + a push to `main`). Changes left in the working tree
(uncommitted) for the founder to commit. One Tier-2 spec ambiguity surfaced and
resolved via Q-IMPL-001 in `content-integrity.md` (the `study` negative token vs.
the D1-mandated "study guides"/"social-studies" phrases).

Per-chunk task status:
- Chunk 0 (build toolchain): t1 ✅ t2 ✅ t3 ✅ t4 ✅ — COMPLETE
- Chunk 1 (Why-Us rewrite): t1 ✅ — COMPLETE
- Chunk 2 (head CDN→link): t1 ✅ — COMPLETE
- Chunk 3 (OG/Twitter+favicon): t1 ✅ t2 ✅ — COMPLETE
- Chunk 4 (deploy workflow): t1 ✅ t2 ✅ — COMPLETE (static checks)
- Chunk 5 (Pages-source switch): t1 ⛔ BLOCKED: human-operator
- Chunk 6 (final verification): t1 ✅ (static parity) t2 ✅ (integrity+body diff)
  t3 ⏸ DEFERRED: live smoke (needs deploy)

Planned — ready for `sdd-implement`. Derived from the three Approved-track specs
(`content-integrity.md`, `build-toolchain.md`, `deploy-pipeline.md`). Note: the
upstream requirements and specs carry `status: Draft` in frontmatter but the
pipeline kickoff treats them as approved; this plan proceeds on that basis (see
Open Questions/Assumptions).

## Overview

One focused cycle on the existing single-page brochure site (`index.html`,
`logo.png`, `CNAME` at repo root). Two independent work streams plus an
integration point: (1) an integrity rewrite of the "Why Us" first paragraph to
the spec-pinned copy, and (2) a technical modernization — replace the Tailwind
CDN with a real Tailwind v4 build, add OG/Twitter meta + favicon, and deploy via
a GitHub Actions Pages workflow with CNAME preservation. The plan pins every
concrete artifact (versions, file paths, the `@theme` block, action versions, the
exact copy) so implementation is unambiguous.

## Conventions

- **Task types**: `[implement]` produces code/files, `[spike]` produces findings
  (may trigger replan), `[verify]` validates behavior against acceptance criteria.
- **Chunk headers**: `### Chunk N: <name>` per work unit.
- **Dependencies**: each chunk states `**Depends on**: Chunk N` (or `None`) so the
  dependency structure is machine-parseable.
- **Acceptance criteria** on every task trace to a REQ/spec ID.

## Shared-file sequencing note (read first)

Chunks 1, 2, and 3 **all edit `index.html`** — Chunk 1 edits the `<body>`
(`#why-us` first `<p>`), Chunks 2 and 3 edit the `<head>`. They are **not**
conflict-free parallel chunks. They are sequenced as an ordered chain on
`index.html`: **Chunk 1 → Chunk 2 → Chunk 3**, each depending on the prior so a
single editor touches the file at a time. Chunk 1 (body) and Chunk 2 (head, CDN
removal) touch disjoint regions and could in principle be merged, but are kept
ordered to keep the body byte-diff (REQ-CONTENT-009) trivially verifiable against
a known pre-image. Chunk 0 (build toolchain files: `package.json`,
`src/input.css`, lockfile — no `index.html` edit) is genuinely independent and may
run in parallel with the `index.html` chain; the `<head>` `<link>` edit that
*depends* on the build output lives in Chunk 2. Chunk 4 (deploy workflow) depends
on Chunk 0 existing. Treat Chunk 0 and the Chunk 1→2→3 chain as the two parallel
streams; Chunks 4–6 are integration/verification.

## Chunks

### Chunk 0: Tailwind v4 build toolchain (no index.html edit)
**Goal**: `npm ci && npm run build:css` produces a non-empty minified
`dist/styles.css` resolving all 14 D4 brand classes. Pure new-file work; does not
touch `index.html`.
**Depends on**: None
**Tasks**:
1. [implement] Add root `package.json` exactly per build-toolchain.md D1: `name`
   `j4d-web`, `private: true`, devDeps `tailwindcss` `^4.3.0` + `@tailwindcss/cli`
   `^4.3.0` (do not regress below 4.3), and `scripts.build:css` =
   `tailwindcss -i ./src/input.css -o ./dist/styles.css --minify`. — traces to
   build-toolchain.md D1 [REQ-DEPLOY-001].
   *Acceptance*: `package.json` parses; both deps pinned `^4.3.0`; `build:css`
   uses `--minify`.
2. [implement] Add `src/input.css` exactly per build-toolchain.md D2:
   `@import "tailwindcss";` then an `@theme` block defining all eight
   `--color-brand-{50:#eff6ff,100:#dbeafe,200:#bfdbfe,500:#3b82f6,600:#2563eb,700:#1d4ed8,800:#1e3a5f,900:#0f2440}`.
   No `@source`; no `tailwind.config.js`. — traces to build-toolchain.md D2
   [REQ-DEPLOY-002].
   *Acceptance*: file contains the import + `@theme` with all 8 pinned shades; no
   `tailwind.config.{js,cjs,mjs}` exists anywhere in the repo.
3. [implement] Run `npm install` once locally to generate and commit
   `package-lock.json` pinning resolved v4 patch versions. — traces to
   build-toolchain.md D1 [REQ-DEPLOY-010].
   *Acceptance*: `package-lock.json` committed; pins `tailwindcss` +
   `@tailwindcss/cli` v4 resolved versions.
4. [verify] Run `npm ci && npm run build:css`; assert exit 0, no warnings, and a
   non-empty `dist/styles.css`; then grep the compiled CSS for all 14 D4 brand
   classes: `.bg-brand-900 .bg-brand-600 .bg-brand-700 .bg-brand-100 .bg-brand-50
   .text-brand-900 .text-brand-200 .text-brand-600 .text-brand-700
   .border-brand-800 .hover\:bg-brand-100:hover .hover\:bg-brand-700:hover
   .hover\:text-brand-200:hover .hover\:text-brand-900:hover` — `.border-brand-800`
   (shade 800's only consumer) and `.hover\:text-brand-900` asserted explicitly. —
   traces to build-toolchain.md D4 / `build_css_compiles`,
   `build_visual_parity_classes` [REQ-DEPLOY-001, 004].
   *Acceptance*: build clean; all 14 classes present in `dist/styles.css`.
**Entry criteria**: None (first chunk).
**Exit criteria**: lockfile committed; local build green; 14-class grep passes.

### Chunk 1: Content integrity — "Why Us" rewrite (index.html body)
**Goal**: `#why-us` first paragraph replaced with the D1-pinned copy; rest of
`<body>` byte-unchanged.
**Depends on**: None (independent of Chunk 0; first link of the `index.html` chain)
**Tasks**:
1. [implement] Replace only the inner sentence text of the first `<p>` (current
   `index.html` lines 174-178) inside `<section id="why-us">` with the exact D1
   copy ("We've talked directly with K-12 teachers about how they actually build
   their tests, worksheets, and study guides — from social-studies and science
   teachers describing exactly what breaks, through to the math and English
   workflows at the edges of what we've heard. Those conversations surfaced the
   real workflows, the real pain points, and the real failure modes of getting AI
   wrong: off-curriculum questions, silent errors, and teachers losing track of
   what's on their own test."). Keep the `<p class="text-lg text-gray-600
   leading-relaxed mb-6">` wrapper, the `<section>`, heading, and the Tracy/San
   Joaquin Valley locality paragraph (lines 180-182) **byte-unchanged**. Em-dash
   may be literal or `&mdash;` per existing file style. — traces to
   content-integrity.md D1, D3 [REQ-CONTENT-001, 007, 008, 009].
   *Acceptance*: paragraph contains whole-word "talked"; names social studies +
   science as the stated pair and frames math/English as "the edges of what we've
   heard"; no class (a)/(b/e)/(c)/(d)/(f) forbidden tokens; locality paragraph and
   all other `<body>` content unchanged.
**Entry criteria**: None.
**Exit criteria**: D1 copy in place; locality paragraph intact; `<body>` diff
confined to the first Why-Us paragraph.

### Chunk 2: Build toolchain — index.html head swap (CDN → compiled CSS)
**Goal**: `index.html` `<head>` no longer loads runtime Tailwind; it links the
compiled stylesheet.
**Depends on**: Chunk 1 (shared `index.html`); Chunk 0 (the `dist/styles.css`
target the new `<link>` references must exist/build)
**Tasks**:
1. [implement] In `index.html` `<head>`: remove the
   `<script src="https://cdn.tailwindcss.com"></script>` (line 9) and the entire
   inline `<script> tailwind.config = { … } </script>` block (lines 10-29); add
   `<link rel="stylesheet" href="dist/styles.css">`. No runtime Tailwind
   dependency may remain. — traces to build-toolchain.md D3 [REQ-DEPLOY-003].
   *Acceptance*: `index.html` contains no `cdn.tailwindcss.com` and no inline
   `tailwind.config`; contains `<link rel="stylesheet" href="dist/styles.css">`.
**Entry criteria**: Chunk 1 complete (body edit landed); Chunk 0 build artifacts
producible.
**Exit criteria**: `build_html_no_cdn` check passes.

### Chunk 3: Head metadata — OG/Twitter meta + favicon (index.html head)
**Goal**: social-share preview and favicon present in `<head>`.
**Depends on**: Chunk 2 (shared `index.html`; sequenced after the head swap)
**Tasks**:
1. [implement] Add to `index.html` `<head>` the nine OG/Twitter meta tags exactly
   per deploy-pipeline.md D4 (`og:type=website`, `og:title`, `og:description`,
   `og:url=https://j4d.net`, `og:image=https://j4d.net/logo.png`,
   `twitter:card=summary`, `twitter:title`, `twitter:description`,
   `twitter:image=https://j4d.net/logo.png`), values mirroring the existing
   `<title>`/`meta description`. — traces to deploy-pipeline.md D4
   [REQ-DEPLOY-008].
   *Acceptance*: head contains og:type/title/description/url/image and
   twitter:card/title/description/image; `og:url` = `https://j4d.net`.
2. [implement] Add `<link rel="icon" href="logo.png">` to `<head>` per
   deploy-pipeline.md D5 (reuses `logo.png`). — traces to deploy-pipeline.md D5
   [REQ-DEPLOY-009].
   *Acceptance*: head contains `<link rel="icon" …>`.
**Entry criteria**: Chunk 2 complete.
**Exit criteria**: `deploy_og_twitter_meta` and `deploy_favicon` checks pass; all
`index.html` edits for this cycle now landed.

### Chunk 4: Deploy pipeline — GitHub Actions Pages workflow
**Goal**: a `.github/workflows/deploy.yml` that builds the CSS and deploys the
assembled `_site/` to Pages, preserving `CNAME`.
**Depends on**: Chunk 0 (workflow runs `npm ci` + `npm run build:css`, which
require `package.json`/lockfile/`src/input.css`)
**Tasks**:
1. [implement] Create `.github/workflows/deploy.yml` per deploy-pipeline.md D1:
   triggers `push` to `main` + `workflow_dispatch`; top-level `permissions`
   `contents: read`, `pages: write`, `id-token: write`; `concurrency` `group:
   pages`, `cancel-in-progress: false`; **build job** (`ubuntu-latest`):
   `actions/checkout@v4`, `actions/setup-node@v4` (`node-version: 20`), `npm ci`,
   `npm run build:css`, assemble step (D2), `actions/upload-pages-artifact@v3`
   with `path: ./_site`; **deploy job** `needs: build`, `environment: { name:
   github-pages, url: ${{ steps.deployment.outputs.page_url }} }`, step `id:
   deployment` `actions/deploy-pages@v4`. Pin versions exactly; do not regress. —
   traces to deploy-pipeline.md D1 [REQ-DEPLOY-005].
   *Acceptance*: `deploy_workflow_shape` — all triggers, permissions, concurrency
   keys, pinned action versions, and two-job shape present.
2. [implement] Author the build job's "Assemble site" step per deploy-pipeline.md
   D2/D3: `mkdir -p _site/dist`; `cp index.html logo.png _site/`;
   `cp dist/styles.css _site/dist/styles.css`; `cp CNAME _site/CNAME`. The
   `cp CNAME _site/CNAME` line is a mandatory hard gate (D3 failure mode). —
   traces to deploy-pipeline.md D2, D3 [REQ-DEPLOY-007, 006].
   *Acceptance*: `deploy_assembles_assets` (index.html, logo.png,
   dist/styles.css, CNAME copied into `_site/`) and `deploy_cname_preserved`
   (`cp CNAME _site/CNAME` present) both pass.
**Entry criteria**: Chunk 0 complete (build is runnable in CI).
**Exit criteria**: workflow file passes `deploy_workflow_shape`,
`deploy_assembles_assets`, `deploy_cname_preserved` static checks.

### Chunk 5: Manual operator step — flip Pages source (HUMAN, non-CI)
**Goal**: Pages serves the Actions artifact and `j4d.net` persists. **This is a
human-operator step, not CI-automatable.**
**Depends on**: Chunk 4 (workflow must exist and have run a first successful
deploy)
**Tasks**:
1. [verify] **HUMAN-OPERATOR (not scriptable, not a CI acceptance criterion):**
   In repo Settings → Pages → Build and deployment → Source, switch "Deploy from a
   branch" → **"GitHub Actions"**. After the first successful deploy, confirm
   Settings → Pages still shows custom domain **`j4d.net`**; if blank, re-enter
   once (it then persists because the artifact carries `CNAME`). — traces to
   deploy-pipeline.md D6 / `deploy_pages_source_switch` [REQ-DEPLOY-011].
   *Acceptance*: operator confirms `j4d.net` resolves and persists post-deploy.
**Entry criteria**: Chunk 4 merged and a deploy has run.
**Exit criteria**: operator-confirmed `j4d.net` live and persistent.

### Chunk 6: Final integration verification
**Goal**: holistic confirmation against acceptance criteria across all specs.
**Depends on**: Chunk 3 (all `index.html` edits), Chunk 0 (build), Chunk 4
(workflow); Chunk 5 for the live-smoke portion
**Tasks**:
1. [verify] Visual parity check: render `index.html` against the compiled
   `dist/styles.css` and confirm the 14-class brand inventory renders equivalent
   to the prior CDN render (nav/hero/contact brand-900 backgrounds, brand-600
   buttons, brand-200 hover text, brand-50/600 service icons, brand-100/700
   pain-point badges). — traces to build-toolchain.md D4 manual check
   [REQ-DEPLOY-004].
   *Acceptance*: all enumerated brand classes render as before; no visual drift.
2. [verify] Integrity token-absence tests scoped to the `#why-us` first paragraph
   ONLY (whole-word, case-insensitive): assert presence of "talked"; assert
   absence of class (a) duration, (b/e) interview/survey/study/call, (c)
   grade-coverage, (d) inflated-count, (f) longitudinal tokens; confirm the
   do-not-flag list is NOT matched — "Book a Call" CTA, the "I spent years…"
   quote, and the "Problems We've Heard From Teachers" heading are out of scope
   and must not register as violations. Also run `content_body_preserved`: byte-
   diff `<body>` old vs new, changes confined to the Why-Us first paragraph. —
   traces to content-integrity.md D2 + Verification / `content_whyus_*`,
   `content_body_preserved` [REQ-CONTENT-001..009].
   *Acceptance*: all `content_whyus_*` automated checks pass with correct scoping;
   `<body>` diff confined to the Why-Us first paragraph.
3. [verify] Live smoke (post-deploy, after Chunk 5): load `https://j4d.net`,
   confirm styled page renders with no CDN request, `logo.png`/favicon load, and a
   link-share preview shows the OG title/description/image. — traces to
   deploy-pipeline.md `deploy_live_smoke` [REQ-DEPLOY-004, 008, 009].
   *Acceptance*: live site styled, favicon + logo load, OG preview correct.
**Entry criteria**: Chunks 0, 3, 4 complete (steps 1–2); Chunk 5 complete (step 3).
**Exit criteria**: all automated checks green; manual parity + live smoke
confirmed.

## Dependency Summary (machine-parseable edges)

- Chunk 0 → (none)
- Chunk 1 → (none)
- Chunk 2 → Chunk 1, Chunk 0
- Chunk 3 → Chunk 2
- Chunk 4 → Chunk 0
- Chunk 5 → Chunk 4
- Chunk 6 → Chunk 0, Chunk 3, Chunk 4, Chunk 5

Two parallel streams from the root: **Chunk 0** (build files) and the
**Chunk 1 → 2 → 3** `index.html` chain. They converge at Chunk 2 (the `<link>`
references Chunk 0's output) and finally at Chunk 4/6. The three `index.html`
chunks are an ordered single-editor chain, not concurrent.

## Replan Triggers

- If `npm run build:css` with Tailwind v4.3 fails to auto-detect `index.html` as a
  source (no brand classes emitted) → add `@source "./index.html";` to
  `src/input.css` (D2 documented fallback); if still failing, replan the build
  approach.
- If any of the 14 D4 brand classes is missing from compiled `dist/styles.css`
  (e.g. a shade not generated from `@theme`) → revisit the `@theme` block / class
  enumeration before proceeding to deploy.
- If `@tailwindcss/cli` v4 has split/renamed the `tailwindcss` bin at
  implementation time such that the `build:css` script does not resolve → adjust
  the script to the documented v4 invocation and re-pin; replan D1 if the package
  split changed.
- If `deploy-pages@v4` / `upload-pages-artifact@v3` are themselves deprecated or
  incompatible at implementation time → re-pin to the current GitHub-recommended
  Pages-action versions and update D1.
- If, after the manual Pages-source switch, `j4d.net` does NOT persist despite
  `CNAME` in the artifact → investigate Pages custom-domain handling; this is a
  hard acceptance gate (D3) and blocks cycle completion.
- If upstream requirements/specs change status or content (they are currently
  `Draft`) → this plan is stale and must be re-derived.

## Risks

- **Custom-domain breakage (high impact):** the Pages source switch makes GitHub
  publish *only* the artifact; a missing `_site/CNAME` reverts `j4d.net` to
  `*.github.io`. Mitigated by the mandatory `cp CNAME _site/CNAME` (Chunk 4 task 2,
  hard gate) and the Chunk 5 operator verification.
- **Visual drift (medium):** CSS-first `@theme` must regenerate every brand
  utility the CDN previously produced. Mitigated by the enumerated 14-class grep
  (Chunk 0 task 4) plus the manual parity check (Chunk 6 task 1).
- **Shared-file edit conflicts (low, sequenced away):** three chunks edit
  `index.html`; mitigated by the Chunk 1→2→3 single-editor ordering.
- **v4 version/CLI churn (low):** version pins may have moved; mitigated by the
  replan triggers above and "do not regress below 4.3".

## Spec Coverage Check

Every spec design section maps to a task:
- content-integrity D1/D3 → Chunk 1; D2 + automated verification → Chunk 6 task 2.
- build-toolchain D1 → Chunk 0 t1/t3; D2 → Chunk 0 t2; D3 → Chunk 2; D4 → Chunk 0
  t4 + Chunk 6 t1.
- deploy-pipeline D1 → Chunk 4 t1; D2/D3 → Chunk 4 t2; D4 → Chunk 3 t1; D5 →
  Chunk 3 t2; D6 → Chunk 5; live smoke → Chunk 6 t3.

No spec section is left uncovered. All 20 requirements (REQ-CONTENT-001..009,
REQ-DEPLOY-001..011) are traced by at least one task.

## Open Questions / Assumptions

- **Upstream `Draft` status:** requirements and all three specs carry
  `status: Draft` in frontmatter while the kickoff treats them as approved.
  Default (taken): plan against them as the source of truth; if they change, this
  plan is stale (see replan triggers). No operator available to confirm.
- **Flat publish layout:** assumed (relative `dist/styles.css`, `logo.png`).
  Default: keep flat; if layout changes, update the Chunk 4 `cp` steps and the
  `<link>`/`<img>` hrefs together.
- **OG image + favicon reuse `logo.png`:** no dedicated assets in source. Default
  per REQ-DEPLOY-008/009: reuse `logo.png` (absolute URL for OG). If a dedicated
  asset is added, extend the Chunk 4 assemble step (REQ-DEPLOY-007).
- **Educator count numberless:** D1 copy states no number by design; if one is
  ever added it must be 12 (REQ-CONTENT-005).
- **`dist/styles.css` not committed:** treated as a CI build artifact (Chunk 0/4);
  local builds are for verification only.
