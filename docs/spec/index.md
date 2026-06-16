---
status: Draft
last_updated: 2026-06-09
---

# Spec Index

## Summary

Design specs for the j4d.net integrity-and-toolchain cycle. Three cohesive
spec areas cover the approved requirements (REQ-CONTENT-001..009,
REQ-DEPLOY-001..011): a content-integrity rewrite of the "Why Us" paragraph, a
Tailwind v4 build toolchain, and a GitHub Actions Pages deploy pipeline. Specs
define contracts and verification criteria, pinning the concrete artifacts that
research RS-001 already settled (config values, action versions, exact head
tags) so implementation is unambiguous.

## Specs

| Spec | File | Status | Requirements |
|------|------|--------|--------------|
| Content Integrity | content-integrity.md | Draft | REQ-CONTENT-001..009 |
| Build Toolchain | build-toolchain.md | Draft | REQ-DEPLOY-001..004, 010 |
| Deploy Pipeline | deploy-pipeline.md | Draft | REQ-DEPLOY-005..009, 011 |

## Requirement → Spec Coverage

| Requirement | Spec |
|-------------|------|
| REQ-CONTENT-001 | content-integrity.md |
| REQ-CONTENT-002 | content-integrity.md |
| REQ-CONTENT-003 | content-integrity.md |
| REQ-CONTENT-004 | content-integrity.md |
| REQ-CONTENT-005 | content-integrity.md |
| REQ-CONTENT-006 | content-integrity.md |
| REQ-CONTENT-007 | content-integrity.md |
| REQ-CONTENT-008 | content-integrity.md |
| REQ-CONTENT-009 | content-integrity.md |
| REQ-DEPLOY-001 | build-toolchain.md |
| REQ-DEPLOY-002 | build-toolchain.md |
| REQ-DEPLOY-003 | build-toolchain.md |
| REQ-DEPLOY-004 | build-toolchain.md |
| REQ-DEPLOY-005 | deploy-pipeline.md |
| REQ-DEPLOY-006 | deploy-pipeline.md |
| REQ-DEPLOY-007 | deploy-pipeline.md |
| REQ-DEPLOY-008 | deploy-pipeline.md |
| REQ-DEPLOY-009 | deploy-pipeline.md |
| REQ-DEPLOY-010 | build-toolchain.md |
| REQ-DEPLOY-011 | deploy-pipeline.md |

## Open Questions / Assumptions (cycle-level)

- **Requirements status is `Draft` in frontmatter** but the pipeline kickoff
  treats them as approved. Default: proceed to write Draft specs against them as
  the source of truth; if requirements change, specs are stale and must be
  re-derived.
- **Flat publish layout** assumed (relative paths `dist/styles.css`,
  `logo.png`). Default: keep flat. If layout changes, update workflow `cp` steps
  and `<link>`/`<img>` hrefs (mirrors requirements Open Questions).
- **OG image and favicon reuse `logo.png`** (no dedicated assets in source).
  Default per REQ-DEPLOY-008/009: reuse `logo.png`.

## See Also

- Requirements: `docs/requirements/index.md`
- Research: `docs/research/RS-001-integrity-build/findings.md`
- Traceability: `docs/requirements/traceability.md`
