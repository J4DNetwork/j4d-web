---
id: RS-001
status: Complete
date: 2026-06-09
questions:
  - "What does the captured discovery evidence actually support about J4D's teacher conversations (distinct educators, subjects, grade levels, capture status, time span)? Output a defensible-claim envelope for the 'Why Us' copy."
  - "Confirm the minimal current (2026) Tailwind-CLI + GitHub Actions Pages deploy recipe, including CNAME preservation and the one-time manual repo setting."
budget: "Light — single research pass (brochure site; SDD value here is integrity verification, not deep exploration)."
---

# Research: j4d.net Integrity + Build Step

## Questions

1. **Integrity ledger** — from the captured artifacts only (no fabrication): how many distinct
   educators have been in direct conversation; which subjects/grade levels are evidenced; capture
   status of each; over what time span. Produce a "defensible claim envelope" for the "Why Us"
   copy and an explicit list of phrases that would overreach.
2. **Build/deploy mechanics** — confirm the current minimal recipe for Tailwind CLI → minified
   static CSS, a GitHub Actions Pages workflow (pinned action versions, required
   permissions/environment), CNAME preservation, and the one-time manual repo setting.

## Findings

### Q1 — Integrity ledger / defensible-claim envelope

**Answer**: The evidence is **real but modest and almost entirely Reddit-public**. It supports
"conversations with teachers across multiple subjects" but does **not** support "months of
researching," "interviews," or any claim implying depth/longitudinal engagement or broad
grade-level coverage.

**The capture ledger** (all pseudonyms exactly as recorded in artifacts; no real names exist in
source). **12 distinct educators** are evidenced across three Reddit posts plus one DM thread:

| Pseudonym | Subject (as evidenced) | Grade band | Capture status |
|-----------|------------------------|------------|----------------|
| Educator-Beta | HS Social Studies (APUSH/WHAP) — explicitly confirmed | High school | Public reply (multi-turn, detailed) + DM accepted; **DM interview in-progress, signals "Awaiting reply"** (int-0001) |
| Educator-Alpha | History | Not specified | Public reply (2 turns) |
| Educator-Lambda | Math (inferred: Desmos/EquatIO) | Not specified | Public reply; our follow-up posted, no further reply captured |
| Educator-Mu | Physics & Chemistry (LaTeX/PGFplots) | Not specified | Public reply (2 turns) |
| Educator-Delta | Not specified | Not specified | Public reply |
| Educator-Epsilon | Not specified | Not specified | Public reply |
| Educator-Zeta | Grammar/English + Literature (Publisher/LaTeX, now admin) | K-12, ex-classroom | Public reply (very detailed, 3+ turns) + DM sent, **no response** |
| Educator-Eta | Not specified | Not specified | Public reply (brief) |
| Educator-Theta | Spans K (letter sounds) to HS freshman (novel) | K through HS | Public reply |
| Educator-Iota | Not specified | Not specified | Public reply (brief) |
| Educator-Gamma | Not specified | Not specified | Public reply (link only) |
| Educator-Kappa | English/Writing (inferred) | Not specified (yr-3 teacher) | Public reply (2 turns) |

Distinct pseudonyms: Alpha, Beta, Gamma, Delta, Epsilon, Zeta, Eta, Theta, Iota, Kappa, Lambda,
Mu = **12 distinct educators**.

**Subjects evidenced (explicit or strongly inferred):** Social Studies/History, Math, Physics &
Chemistry, English/Writing/Grammar, Literature — **~5 subject areas**. Only 2 are explicitly
stated by the teacher (Social Studies APUSH/WHAP; Physics/Chemistry); the rest are inferred from
tool/context cues.

**Grade levels evidenced:** Only **2 explicit grade-band data points** — Educator-Beta (high
school) and Educator-Theta (examples spanning kindergarten through high-school freshman). Most
respondents did not state a grade level. "Across grade levels" is therefore **thinly** supported.

**Capture status summary:**
- **0** completed depth interviews. The single DM interview (int-0001 / Educator-Beta) is
  `status: in-progress`; its Signals section reads literally "_Awaiting reply._"
- **2** DM outreaches sent (Educator-Beta accepted then went quiet at the question stage;
  Educator-Zeta: no response).
- **~12** public Reddit replies, several multi-turn, captured verbatim with analysis.

**Time span:** Reddit posts created **2026-04-27**; replies/follow-ups captured **2026-04-28**;
the one DM exchange dated **2026-05-14**. Captured evidence spans roughly **late April to mid-May
2026 — about 2-3 weeks**, not months. (Repo commits run 2026-04-27 → 2026-06-09, but that is repo
activity, not research duration.)

**Evidence strength self-labeled in artifacts:** both segment files (seg-0001, seg-0002) carry
`evidence_strength: anecdotal`.

**Confidence**: High. Every claim is traceable to a specific artifact line.

**Evidence (source paths):**
- `companies/j4d-network/01-ideation/_guides/reddit-post-k12-prep-workflow.md` (Beta, Alpha, Lambda, Mu)
- `companies/j4d-network/01-ideation/_guides/reddit-post-k12-quality-signals.md` (Delta, Epsilon, Zeta, Eta, Theta, Iota)
- `companies/j4d-network/01-ideation/_guides/reddit-post-k12-test-decisions.md` (Gamma, Kappa)
- `companies/j4d-network/01-ideation/interviews/int-0001.md` (Beta DM — in-progress, "Awaiting reply")
- `companies/j4d-network/01-ideation/segments/seg-0001.md`, `seg-0002.md` (`evidence_strength: anecdotal`)
- Dates: post frontmatter `created: 2026-04-27`/`2026-04-28`; int-0001 `date: 2026-05-14`.

#### DEFENSIBLE CLAIM ENVELOPE (the deliverable)

> **Backed by evidence:** "We've talked directly with K-12 teachers about how they actually build
> their tests, worksheets, and study guides — across subjects from social studies and English to
> math, physics, and chemistry. Those conversations surfaced the real workflows, the real pain
> points, and the real failure modes of getting AI wrong (off-curriculum questions, silent errors,
> teachers losing track of what's on their own test)."

This is the strongest statement fully inside the evidence: ~12 distinct teachers, multi-subject,
verbatim-captured, with concrete pain/failure detail. It avoids duration and depth claims.

**Phrases that OVERREACH (must NOT appear in the rewrite), with reason:**

- **"We've spent months researching"** — UNSUPPORTED. Captured evidence spans ~2-3 weeks
  (2026-04-27 → 2026-05-14). "Months" is fabrication.
- **"direct conversations with teachers"** if it implies *interviews/calls* — OVERREACH. All but
  one exchange are public Reddit comment threads; the single DM "conversation" is in-progress with
  **zero answers captured** ("Awaiting reply"). Use "talked with / heard directly from teachers";
  avoid "interviews," "we interviewed," "calls."
- **"across subjects and grade levels"** implying broad grade coverage — PARTIAL OVERREACH.
  "Across subjects" is fine (~5 subject areas). "Across grade levels" rests on **only 2 explicit
  grade-band data points**. Prefer dropping/softening grade-level breadth; "K-12 teachers"
  generically is fine, but do not claim systematic grade coverage.
- **"dozens of teachers" / large numbers** — UNSUPPORTED. Defensible number is ~12; do not inflate.
- **"research study," "we surveyed," "data shows"** — OVERREACH. Self-labeled `anecdotal`; no
  survey instrument, no quantitative data.
- **"longitudinal / over years / deep relationships"** — UNSUPPORTED. Single-touch public replies.

**Net guidance for the spec stage:** keep "direct conversations with teachers" + multi-subject;
**delete "months,"** soften/drop grade-level breadth, never imply formal interviews/surveys.

### Q2 — Build/deploy mechanics

**Operator decision (resolved):** Pin **Tailwind v4** — confirmed as the current stable major as of
June 2026 (latest stable is **v4.3.0**, published 2026-05-08; v4 line: 4.0 → 4.1 → 4.2 → 4.3). The
prior v3.4 recipe (JS `tailwind.config.js` + `tailwindcss` npm CLI) is **SUPERSEDED**. v4 changed
the model substantially: CSS-first config, a separate `@tailwindcss/cli` package, and automatic
source detection.

**Answer**: Compile `index.html`'s classes into a minified `dist/styles.css` with the v4
`@tailwindcss/cli`, define the `brand` palette **in CSS** via the `@theme` directive (no
`tailwind.config.js`), and deploy via a GitHub Actions workflow using
`actions/upload-pages-artifact@v3` + `actions/deploy-pages@v4`. CNAME is preserved by copying it
into the published artifact directory. One manual repo setting (Pages → Source → GitHub Actions) is
required once.

**Confidence**: High. v4 package name, CSS-first `@theme` color mapping, automatic source
detection, the `--minify` flag, action versions, and CNAME mechanics all confirmed against current
Tailwind docs (tailwindcss.com, June 2026) and GitHub docs/repos.

#### Tailwind classes/config in use (inventoried from `index.html`)
- Custom palette needed: `brand.{50,100,200,500,600,700,800,900}` (eight shades; values from the
  current inline `<script>` config) — moved into a CSS `@theme` block as `--color-brand-*`.
- Brand classes used: `bg-brand-{900,600,700,100,50}`, `text-brand-{900,200,600,700}`,
  `hover:bg-brand-{100,700}`, `hover:text-brand-200`. In v4 these are generated automatically from
  the `--color-brand-*` custom properties (e.g. `--color-brand-600` → `bg-brand-600`,
  `text-brand-600`, `border-brand-600`, etc.).
- All other utilities are core Tailwind (layout, typography, `grid md:grid-cols-3`, `sm:`/`md:`
  variants, `hover:shadow-lg`, `transition`, `antialiased`, `rounded-*`). No plugins required.
- **Source glob: not needed.** v4 auto-detects sources (scans all non-ignored, non-binary,
  non-CSS, non-lockfile files). For a single root `index.html` built via the CLI, **no `@source`
  directive is required** — `index.html` is detected automatically. (`@source` would only be needed
  to scan `node_modules`, adjust a monorepo base path, exclude legacy dirs, or safelist.)

#### Recipe (version-pinned, Tailwind v4)

**1. Project files**

`package.json` (npm route — most reproducible in CI):
```json
{
  "devDependencies": {
    "tailwindcss": "^4.3.0",
    "@tailwindcss/cli": "^4.3.0"
  },
  "scripts": {
    "build:css": "tailwindcss -i ./src/input.css -o ./dist/styles.css --minify"
  }
}
```
Notes: v4 splits the CLI into its own package — `@tailwindcss/cli` provides the `tailwindcss`
binary, and the `tailwindcss` package is the engine both depend on; install both as devDeps. Keep
both pinned to the same minor (`^4.3.0`). The npm-script `tailwindcss -i … -o … --minify` resolves
to the `@tailwindcss/cli` bin; equivalently `npx @tailwindcss/cli -i … -o … --minify`. There is **no
`tailwind.config.js`** in v4 for this project.

`src/input.css` (CSS-first config — replaces both `@tailwind` directives **and**
`tailwind.config.js`):
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
v4 generates `bg-brand-600`, `text-brand-900`, `hover:bg-brand-700`, etc. directly from these
`--color-brand-*` theme variables — no JS config and no `content`/`extend` block.

In `index.html`: remove `<script src="https://cdn.tailwindcss.com">` **and** the inline
`tailwind.config` `<script>` (neither applies in v4 — the CDN script is dev-only and the JS config
is gone); add `<link rel="stylesheet" href="dist/styles.css">`.

> Standalone-CLI alternative (no Node): download the v4 `tailwindcss` standalone binary release and
> run `./tailwindcss -i src/input.css -o dist/styles.css --minify`. Same input CSS / `@theme`
> block; npm route recommended for CI reproducibility (pinned version).

**2. GitHub Actions workflow** — `.github/workflows/deploy.yml`:
```yaml
name: Deploy to GitHub Pages
on:
  push:
    branches: [main]
  workflow_dispatch:

permissions:
  contents: read
  pages: write
  id-token: write

concurrency:
  group: pages
  cancel-in-progress: false

jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: 20
      - run: npm ci
      - run: npm run build:css      # v4 @tailwindcss/cli -> dist/styles.css --minify
      - name: Assemble site
        run: |
          mkdir -p _site/dist
          cp index.html logo.png _site/
          cp dist/styles.css _site/dist/styles.css
          cp CNAME _site/CNAME          # <-- CNAME preservation
      - uses: actions/upload-pages-artifact@v3
        with:
          path: ./_site

  deploy:
    needs: build
    runs-on: ubuntu-latest
    environment:
      name: github-pages
      url: ${{ steps.deployment.outputs.page_url }}
    steps:
      - id: deployment
        uses: actions/deploy-pages@v4
```

Notes: `npm ci` requires a committed `package-lock.json` (run `npm install` once locally). For the
standalone binary, replace setup-node/npm steps with a download + `./tailwindcss ... --minify` step.

**3. CNAME preservation (key gotcha):** When Pages source is switched from "deploy from branch" to
"GitHub Actions," GitHub **ignores any repo-root CNAME** and publishes *exactly* the uploaded
artifact. So the existing `CNAME` (contents `j4d.net`) must be **copied into the artifact root**
(`cp CNAME _site/CNAME`). With CNAME present in the artifact, `j4d.net` keeps resolving and the
custom-domain setting persists. (GitHub Docs "Troubleshooting custom domains" + multiple community
discussions on CNAME being dropped on Actions deploys.)

**4. One-time manual repo setting (human must do):** **Settings → Pages → Build and deployment →
Source → "GitHub Actions"** (from "Deploy from a branch"). UI-only, not workflow-scriptable. After
the first run, confirm Settings → Pages still shows custom domain `j4d.net`; if blank, re-enter it
once (it then sticks because the artifact carries CNAME).

**Evidence:** tailwindcss.com docs (CLI install — `npm install tailwindcss @tailwindcss/cli`; input
CSS `@import "tailwindcss";`; `--minify` flag; theme — `@theme { --color-* }` generates color
utilities; source detection — automatic, `@source` not needed for a single root `index.html`);
Tailwind release history (latest stable **v4.3.0**, 2026-05-08); `github.com/actions/upload-pages-artifact`
(v3 current); `github.com/actions/deploy-pages` (v4 current; must be v4+ to consume upload v3
artifacts, per GitHub Dec-2024 artifacts-v4 deprecation notice); GitHub Docs custom-domain
troubleshooting.

## Implications for Design
- **"Why Us" rewrite (spec input):** use the defensible-claim envelope; spec acceptance criteria
  should assert the copy contains none of the 6 overreach phrase classes (especially "months").
  This is the integrity gate for the cycle.
- **Verb lock — keep "talked" (M1):** the envelope copy uses **"talked directly with"**. Spec
  acceptance MUST preserve the verb **"talked"** (or "talked with / heard directly from"). The copy
  must **not** drift to "spoke with," "interviewed," "we interviewed," or "had conversations/calls
  with" — those re-introduce the interview/depth overreach the ledger rules out. Treat "talked" as a
  literal, testable acceptance token, not a stylistic choice.
- **Inferred subjects are the soft boundary, not the center (M2):** only **2 of 5** subjects are
  teacher-STATED — social studies (APUSH/WHAP) and physics/chemistry. English and math are
  **INFERRED from tool cues** (Desmos/EquatIO; Publisher/LaTeX, "yr-3 teacher"), not stated by the
  teacher. The defensible claim is *centered* on the two stated subjects; the inferred ones mark the
  **soft outer edge** of the claim envelope. Copy may name the multi-subject range, but spec/acceptance
  should not present math/English as confirmed teacher statements, and any subject-count emphasis
  should lean on the stated pair.
- **Build step:** viable, low-risk. Adds `package.json`, `package-lock.json`, `src/input.css`
  (CSS-first `@theme` config — **no `tailwind.config.js`** in v4),
  `.github/workflows/deploy.yml`; modifies `index.html` (CDN + inline-config scripts → `<link>`).
  `dist/styles.css` need not be committed (CI builds it).
- **Pinned versions:** `actions/checkout@v4`, `actions/setup-node@v4`,
  `actions/upload-pages-artifact@v3`, `actions/deploy-pages@v4`, **Tailwind v4** (`tailwindcss ^4.3`
  + `@tailwindcss/cli ^4.3`), Node 20.
- Kickoff's OG/Twitter meta + favicon are out of scope for this research but trivial static
  additions; **(m2)** ensure any new asset (favicon, OG image) is also added to the workflow's `cp`
  step into `_site/` (alongside `index.html`, `logo.png`), or it will be missing from the published
  artifact.

## Open Questions / Assumptions
- **Tailwind major — RESOLVED: v4.** Operator chose v4; confirmed current stable major is v4
  (latest **v4.3.0**, June 2026). Recipe rewritten for v4 (CSS-first `@theme`, `@tailwindcss/cli`,
  automatic source detection). No `tailwind.config.js`. This is no longer an open question.
- **Publish layout / asset paths.** Assumed flat publish with current relative paths
  (`dist/styles.css`, `logo.png`). If paths change, update `cp` steps and `<link>`/`<img>` hrefs.
- **Distinct-educator count** settled at **12**. Copy should ideally state no number; if one is
  used, it is 12.
- **`package-lock.json`** must be generated/committed for `npm ci` — one-time local `npm install`,
  done during implementation.

## Recommended Next Step
**Proceed to requirements/specs.** Both questions answered with high confidence. The
defensible-claim envelope is concrete enough to drive the "Why Us" rewrite; the deploy recipe is
version-pinned (Tailwind v4), CNAME-safe, and verified against current docs. No open operator
decisions remain.
