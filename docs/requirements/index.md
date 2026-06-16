---
version: 1.0
status: Approved
last_updated: 2026-06-09
---

# Requirements Index

## Summary

j4d.net is a single-page brochure site for J4D Network's K-12 AI-consulting
practice. This requirements set covers one focused cycle that keeps the existing
positioning and fixes two classes of issue: (1) **copy integrity** — rewriting
the "Why Us" claim to stay inside the defensible-claim envelope established by
research RS-001, and (2) **technical modernization** — replacing the Tailwind
CDN with a Tailwind v4 build step, deploying via GitHub Actions to Pages (with
CNAME preservation), and adding Open Graph / Twitter Card meta plus a favicon.
This is an integrity-and-toolchain pass, not a repositioning.

## Stakeholders

- **Operator / site owner** — converged the scope in DISCUSS and approved the
  kickoff; owns the one-time manual Pages-source switch.
- **Research RS-001** — supplies the defensible-claim envelope (Q1) and the
  version-pinned deploy recipe (Q2) that these requirements trace to.

## Files

| Category | File | Domain | Status | Count | Last Updated |
|----------|------|--------|--------|-------|--------------|
| Content | content/integrity.md | CONTENT | Draft | 9 | 2026-06-09 |
| Build/Deploy | build-deploy/toolchain-and-deploy.md | DEPLOY | Draft | 11 | 2026-06-09 |

## Domain Prefixes

| Prefix | File | Description |
|--------|------|-------------|
| CONTENT | content/integrity.md | "Why Us" copy-integrity constraints |
| DEPLOY | build-deploy/toolchain-and-deploy.md | Tailwind v4 build, Pages deploy, CNAME, meta, favicon |

## Out of Scope

- Repositioning the site around the free typeset utility (brief-0001).
- Any consulting-vs-product strategy change.
- New sections or content beyond the integrity rewrite.
- Changes to the hero, services, pain-points sections, or CTAs.
- Analytics, A/B testing, CMS.

## Open Questions

- **Publish layout / asset paths** — requirements assume a flat publish with
  current relative paths (`dist/styles.css`, `logo.png`). Default: keep flat
  layout. If paths change at implementation, update the workflow `cp` steps and
  `<link>`/`<img>` hrefs accordingly.
- **OG image asset** — REQ-DEPLOY-008 requires `og:image`; no dedicated share
  image exists in source. Default assumption: reuse `logo.png` as the OG image
  unless a dedicated image is introduced (in which case REQ-DEPLOY-007 applies).
- **Favicon asset** — default assumption (REQ-DEPLOY-009): reuse `logo.png`
  rather than introduce a dedicated `.ico`/`.png` icon.
- **Stated educator count in copy** — research settled the defensible count at
  12; default is to state no number at all (REQ-CONTENT-005).

No `[needs-spike]` items remain — RS-001 answered both research questions with
high confidence.

## Glossary

- **Defensible-claim envelope** — the RS-001 paragraph defining the strongest
  "Why Us" statement fully supported by captured evidence; the rewrite must stay
  inside it.
- **Overreach phrase classes** — six categories of claim RS-001 flagged as
  unsupported (duration, interview/survey, systematic grade coverage, inflated
  counts, study/quantitative framing, longitudinal/deep-relationship).
- **Verb lock** — the requirement to use "talked" and not drift to
  "spoke/interviewed" (RS-001 note M1).
- **CNAME preservation** — copying the repo-root `CNAME` into the published
  artifact so `j4d.net` survives the Pages source switch to GitHub Actions.

## See Also
- [Traceability Matrix](traceability.md)
- Research: `docs/research/RS-001-integrity-build/findings.md`
- Kickoff: `docs/handoff/kickoff.md`
