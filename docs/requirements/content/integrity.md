---
domain: CONTENT
last_updated: 2026-06-09
status: Draft
---

# Requirements: Why-Us Copy Integrity

## Overview

This cycle keeps j4d.net's existing K-12 AI-consulting positioning and rewrites
only the "Why Us" section so its claims stay inside the **defensible-claim
envelope** established by research RS-001. The current copy (`index.html`
lines 173-179) overreaches: it asserts "We've spent months researching" and
"direct conversations with teachers across subjects and grade levels." RS-001
found the underlying evidence is real but modest — ~12 distinct educators,
mostly public Reddit replies plus one in-progress DM with zero captured
answers, spanning ~2-3 weeks, self-labeled `anecdotal`. These requirements
constrain the rewrite to what the evidence supports. This is an integrity
correction, not a repositioning.

Traceability: all requirements below derive from RS-001 findings.md Q1 (the
defensible-claim envelope and the 6 overreach phrase classes) and the
"Implications for Design" review notes M1 (verb lock) and M2 (inferred-subject
soft boundary).

## Requirements

**Scope of the negative phrase-class criteria.** The NEGATIVE
phrase-class criteria in REQ-CONTENT-002 through REQ-CONTENT-006 apply to
the rewritten "Why Us" `<section id="why-us">` paragraph copy **ONLY** —
not to the whole document. They must not be evaluated against in-scope
unchanged copy that REQ-CONTENT-009 explicitly preserves and that is out
of scope for this rewrite, including: the preserved "Book a Call" CTA; the
pain-point teacher quote "I spent years getting my handwritten tests into
Word"; and the "Problems We've Heard From Teachers" heading. Matching any
of these forbidden phrase classes outside the rewritten Why-Us paragraph
copy is a false positive, not a violation.

### REQ-CONTENT-001: Reworded "Why Us" copy stays inside the defensible-claim envelope
The "Why Us" section copy in `index.html` must be rewritten to assert only
claims supported by RS-001's defensible-claim envelope: direct (but informal)
contact with K-12 teachers about how they build tests, worksheets, and study
guides; multi-subject range; and concrete pain points / failure modes of
getting AI wrong. The copy must not assert research depth, duration, or
formality beyond what the evidence supports.
[Priority: must]
[Traceability: RS-001 Q1 "DEFENSIBLE CLAIM ENVELOPE"]

### REQ-CONTENT-002: No duration / "months of research" claims
The reworded copy must not contain any phrase claiming a duration or span of
research effort — specifically it must not contain "months" or any equivalent
("years," "spent N months/years," "longstanding"). Captured evidence spans
~2-3 weeks; duration claims are unsupported.
[Priority: must]
[Traceability: RS-001 Q1 overreach class (a); review-note "delete months"]

### REQ-CONTENT-003: No interview / survey / study framing
The reworded copy must not contain any of these tokens or their close
variants: "interview," "interviews," "we interviewed," "calls," "surveyed,"
"survey," "study," "research study," or "data shows." All but one exchange were
public Reddit comment threads; the single DM is in-progress with zero captured
answers; segments are self-labeled `anecdotal`. There is no survey instrument
or quantitative dataset.

Token matching is **whole-word and case-insensitive**: the "calls" token
matches "call" / "calls" as standalone words only, and must **not** match
the substring "Call" inside the preserved "Book a Call" CTA label. The
intent (forbid implying phone or interview *calls* in the Why-Us copy)
is unchanged.
[Priority: must]
[Traceability: RS-001 Q1 overreach classes (b) and (e)]

### REQ-CONTENT-004: No systematic grade-level coverage claim
The reworded copy must not assert systematic coverage "across grade levels" or
any equivalent claiming broad/per-grade rigor. Only 2 explicit grade-band data
points exist. Referring generically to "K-12 teachers" is permitted; claiming
systematic grade coverage is not.
[Priority: must]
[Traceability: RS-001 Q1 overreach class (c)]

### REQ-CONTENT-005: No inflated educator counts
The reworded copy must not contain inflated count language such as "dozens" (or
larger). If any number is stated at all, it must be 12 (the defensible distinct
count); a numberless statement is preferred.
[Priority: must]
[Traceability: RS-001 Q1 overreach class (d); Open Questions "count settled at 12"]

### REQ-CONTENT-006: No longitudinal / deep-relationship claims
The reworded copy must not contain "longitudinal," "over years," "deep
relationships," or equivalents implying sustained or repeated engagement. The
evidence is predominantly single-touch public replies.
[Priority: must]
[Traceability: RS-001 Q1 overreach class (f)]

### REQ-CONTENT-007: Verb lock — use "talked"
The reworded copy must use the verb "talked" (e.g., "talked directly with" or
"talked with / heard directly from") to describe contact with teachers. It must
not substitute "spoke with," "interviewed," "we interviewed," or "had
conversations/calls with," which re-introduce interview/depth overreach.
"talked" is a literal, testable acceptance token.
[Priority: must]
[Traceability: RS-001 Implications review-note M1 (verb lock)]

### REQ-CONTENT-008: Inferred subjects treated as the soft boundary
The reworded copy may reference a multi-subject range but must not present the
inferred subjects (English, math) as confirmed teacher statements. Only social
studies (APUSH/WHAP) and physics/chemistry are teacher-stated; any
subject-emphasis must lean on that stated pair, with inferred subjects marking
the soft outer edge of the claim — not asserted with per-subject rigor.
[Priority: must]
[Traceability: RS-001 Implications review-note M2 (inferred-subject soft boundary)]

### REQ-CONTENT-009: Positioning, services, and CTAs unchanged
The rewrite must change only the "Why Us" section's claim copy (and may keep its
Tracy/San Joaquin Valley locality line). The K-12 AI-consulting positioning,
the hero, the services section, the pain-points section, all CTAs (Book a
Conversation / Book a Call / Email Us), and contact details must remain
unchanged. This is integrity, not repositioning.
[Priority: must]
[Traceability: kickoff.md "Out of scope"; RS-001 scope]
