---
id: kickoff-j4d-web-integrity-build
type: research-kickoff
status: active
created: 2026-06-09
target_repo: /Users/pankaj/work/j4dnetwork/j4d-web
cycle: j4d.net integrity + build-step pass
---

# Kickoff: j4d.net integrity + build-step pass

## Idea (converged in DISCUSS)

Keep the existing **K-12 AI-consulting** positioning of j4d.net. Fix two classes
of issue surfaced in review: (1) a copy **integrity** overreach, and (2) **technical**
modernization — replace the Tailwind CDN with a real build step, deployed via
GitHub Actions to Pages, plus OG/Twitter meta and a favicon.

Explicitly NOT in scope: repositioning the site around the free typeset utility
(brief-0001); any consulting-vs-product strategy change.

## Research questions (time-boxed — this is a small change)

1. **Integrity ledger.** What does the captured evidence in
   `/Users/pankaj/work/j4dnetwork/company-os/companies/j4d-network` actually
   support about teacher conversations — how many distinct educators, which
   subjects, which grade levels, capture status (replied vs. awaiting)? Output: a
   one-paragraph "defensible claim envelope" the reworded "Why Us" copy must stay
   inside. Pseudonyms only; do not copy real names.
2. **Build/deploy mechanics.** Confirm the minimal, current path for: Tailwind CLI
   compiling `index.html` classes → minified `dist/styles.css`; a GitHub Actions
   workflow that builds and deploys to Pages via `actions/upload-pages-artifact` +
   `actions/deploy-pages`; and **CNAME preservation** in the published artifact.
   Note the one-time manual "Pages source → GitHub Actions" repo setting.

## Success criteria

- A defensible-claim envelope concrete enough to finalize the "Why Us" rewrite.
- A confirmed, version-pinned deploy recipe (action versions, steps, CNAME handling).
- Known unknowns/assumptions recorded, not guessed.

## Budget

Light: a single research pass. This is a one-page brochure site; SDD's value here
is integrity verification + spec discipline, not deep exploration.

## Out of scope

Utility/product repositioning, consulting strategy, new sections/content beyond
the integrity rewrite, analytics, A/B testing, CMS.
