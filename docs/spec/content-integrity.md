---
status: Approved
last_updated: 2026-06-09
requires: [REQ-CONTENT-001, REQ-CONTENT-002, REQ-CONTENT-003, REQ-CONTENT-004, REQ-CONTENT-005, REQ-CONTENT-006, REQ-CONTENT-007, REQ-CONTENT-008, REQ-CONTENT-009]
---

# Content Integrity — "Why Us" Rewrite

## Context

The current "Why Us" copy (`index.html` lines 173-179) overreaches relative to
the captured discovery evidence: it asserts "We've spent months researching" and
"direct conversations with teachers across subjects and grade levels." RS-001
established that the evidence is real but modest — ~12 distinct educators, mostly
public Reddit replies plus one in-progress DM with zero captured answers,
spanning ~2-3 weeks, self-labeled `anecdotal`. This spec defines the exact
replacement paragraph copy that sits inside RS-001's defensible-claim envelope,
and the checkable verification criteria that enforce the integrity constraints
(REQ-CONTENT-001..008). It also pins what must remain byte-unchanged
(REQ-CONTENT-009). This is an integrity correction, not a repositioning.

## Design

### D1. Replacement copy contract (REQ-CONTENT-001, 007, 008)

The `<p class="text-lg text-gray-600 leading-relaxed mb-6">` paragraph inside
`<section id="why-us">` is replaced with the following final copy. The second
paragraph (the Tracy / San Joaquin Valley locality line, lines 180-182) is
**preserved unchanged**.

**Final "Why Us" paragraph copy (replaces lines 174-178):**

> We've talked directly with K-12 teachers about how they actually build their
> tests, worksheets, and study guides — from social-studies and science teachers
> describing exactly what breaks, through to the math and English workflows at
> the edges of what we've heard. Those conversations surfaced the real workflows,
> the real pain points, and the real failure modes of getting AI wrong:
> off-curriculum questions, silent errors, and teachers losing track of what's on
> their own test.

Rationale for wording choices:

- **Verb lock (REQ-CONTENT-007):** opens with "talked directly with" — the
  literal acceptance token "talked" is present; no "spoke," "interviewed," or
  "had conversations/calls with."
- **Stated vs. inferred subjects (REQ-CONTENT-008):** social studies and science
  (physics/chemistry) are the teacher-STATED pair and carry the concrete clause
  ("describing exactly what breaks"); math and English are explicitly framed as
  "the edges of what we've heard" — the soft outer boundary, never asserted with
  per-subject rigor.
- **Envelope fidelity (REQ-CONTENT-001):** multi-subject range, concrete pain
  points and failure modes, no claim of research depth, duration, or formality.
- **Generic K-12 reference** is retained (permitted by REQ-CONTENT-004); no
  "across grade levels" breadth claim.
- **No stated number (REQ-CONTENT-005):** numberless by design; if a number were
  ever added it must be 12.

The HTML wrapper (tag, classes, the surrounding `<section>`, heading, and
locality paragraph) is unchanged; only the inner sentence text of the first
paragraph changes. Em-dashes/colons may be rendered as literal characters or
HTML entities (`&mdash;`) consistent with the existing file style.

### D2. Negative phrase-class constraints — scope and tokens (REQ-CONTENT-002..006)

The negative criteria apply to the **rewritten Why-Us paragraph copy ONLY** —
the visible text of the first `<p>` inside `<section id="why-us">`. They are
**not** evaluated against the rest of the document. Specifically, the following
preserved strings are explicitly out of scope and must NOT be flagged as
violations (matching them elsewhere is a false positive):

- the "Book a Call" CTA label (the standalone word "Call");
- the pain-point quote "I spent years getting my handwritten tests into Word";
- the "Problems We've Heard From Teachers" heading.

Forbidden token classes (whole-word, case-insensitive) within the Why-Us
paragraph copy:

| Class | REQ | Forbidden tokens (whole-word, case-insensitive) |
|-------|-----|--------------------------------------------------|
| (a) Duration | 002 | `months`, `years`, `month`, `year`, `longstanding` |
| (b/e) Interview/survey/study | 003 | `interview`, `interviews`, `interviewed`, `call`, `calls`, `survey`, `surveyed`, `study`, `data shows` |
| (c) Systematic grade coverage | 004 | `across grade levels`, `grade levels`, `per-grade` |
| (d) Inflated counts | 005 | `dozens`, `hundreds`, `thousands` (and any count > 12) |
| (f) Longitudinal / deep-relationship | 006 | `longitudinal`, `over years`, `deep relationships`, `long-term relationships` |

Matching is **whole-word and case-insensitive**: e.g. the `call`/`calls` tokens
match standalone words only and must NOT match the substring "Call" inside the
preserved CTA, nor any occurrence outside the Why-Us paragraph. The D1 copy
above contains none of these tokens by construction.

Rationale: RS-001 found duration is ~2-3 weeks (not months), all but one
exchange were public Reddit threads (no interviews/surveys/calls), only 2
explicit grade-band data points (no systematic grade coverage), the defensible
count is ~12 (not dozens), and the evidence is single-touch public replies (not
longitudinal).

### D3. Preservation contract (REQ-CONTENT-009)

Everything except the single Why-Us first paragraph remains **byte-unchanged** by
the content rewrite, including: the K-12 AI-consulting positioning, the hero
(nav + header), the services section (all three cards), the pain-points section
(heading + all four items), all CTAs (Book a Conversation / Book a Call / Email
Us), contact details, footer, and the Why-Us locality paragraph (lines 180-182).

Note: the separate build-toolchain and deploy specs modify the `<head>` (CDN
removal, `<link>`, meta, favicon). Those `<head>` edits are governed by
REQ-DEPLOY-* and are out of scope for the REQ-CONTENT-009 body-preservation
contract; this spec's preservation contract covers the document `<body>`.

## Verification

### Automated

- **content_whyus_uses_talked**: the Why-Us first paragraph text contains the
  whole word "talked" (case-insensitive). [REQ-CONTENT-007]
- **content_whyus_subject_boundary**: the paragraph names social studies and
  science as the grounded/stated pair and frames math/English as the soft edge
  ("edges of what we've heard" or equivalent), not as confirmed per-subject
  statements. [REQ-CONTENT-008]
- **content_whyus_no_duration**: paragraph contains none of class (a) tokens.
  [REQ-CONTENT-002]
- **content_whyus_no_interview_survey**: paragraph contains none of class (b/e)
  tokens; the test scope is the Why-Us paragraph only and explicitly excludes the
  "Book a Call" CTA, the "I spent years…" quote, and the "Problems We've Heard
  From Teachers" heading. [REQ-CONTENT-003]
- **content_whyus_no_grade_coverage**: paragraph contains no "across grade
  levels"/"grade levels"/"per-grade"; generic "K-12" is allowed.
  [REQ-CONTENT-004]
- **content_whyus_no_inflated_count**: paragraph contains no "dozens"/larger; if
  any digit count appears it equals 12. [REQ-CONTENT-005]
- **content_whyus_no_longitudinal**: paragraph contains none of class (f) tokens.
  [REQ-CONTENT-006]
- **content_whyus_inside_envelope**: paragraph asserts only direct/informal
  contact, multi-subject range, and concrete pain points/failure modes — no
  research-depth/duration/formality claim. [REQ-CONTENT-001]
- **content_body_preserved**: a byte-diff of the `<body>` between old and new
  `index.html` shows changes confined to the Why-Us first paragraph; hero,
  services, pain-points, all CTAs, contact, footer, and the locality paragraph
  are identical. [REQ-CONTENT-009]

All token tests are whole-word, case-insensitive, and scoped to the rewritten
Why-Us paragraph copy.

### Manual

- Read the rendered Why-Us section and confirm it reads naturally and that the
  social-studies/science emphasis with math/English at the edge matches the
  RS-001 soft-boundary guidance.

### Acceptance Criteria

- [ ] Why-Us first paragraph replaced with the D1 copy; locality paragraph
      unchanged. [REQ-CONTENT-001]
- [ ] Paragraph contains "talked"; no interview/depth verbs. [REQ-CONTENT-007]
- [ ] No duration tokens. [REQ-CONTENT-002]
- [ ] No interview/survey/study/call tokens in the paragraph (CTA/quote/heading
      not flagged). [REQ-CONTENT-003]
- [ ] No systematic grade-coverage claim. [REQ-CONTENT-004]
- [ ] No inflated count (numberless preferred; else 12). [REQ-CONTENT-005]
- [ ] No longitudinal/deep-relationship claim. [REQ-CONTENT-006]
- [ ] Stated subjects (social studies, science) centered; math/English at the
      soft edge. [REQ-CONTENT-008]
- [ ] `<body>` diff confined to the Why-Us first paragraph. [REQ-CONTENT-009]

## Implementation Questions

### Q-IMPL-001: `study` token vs. the D1-mandated "study guides" / "social-studies" phrases
**Tier**: 2 (spec ambiguity)
**Spec reference**: §D2 class (b/e) forbidden token `study` vs. §D1 pinned copy
("tests, worksheets, and **study guides**" and "**social-studies** and science")
and D2's assertion that "The D1 copy above contains none of these tokens by
construction."
**Decision**: The class (b/e) `study` token is matched in its
**research-formality sense only** (a "study"/"studies" as a research instrument,
alongside `interview`/`survey`). The two occurrences in the D1 copy — the teacher
artifact noun phrase "study guides" (a deliverable, parallel to "tests" and
"worksheets") and the school subject "social-studies" (where `grep -w` treats the
hyphen as a word boundary so "studies" matches) — are explicitly excluded, exactly
as D2 already excludes the substring "Call" in the "Book a Call" CTA. The
integrity scan strips "study guides" and "social-studies" before testing for a
research-sense `study`/`studies`; none remains. A naive bare whole-word `study`
matcher produces a false positive on the spec's own pinned, correct D1 text.
**Rationale**: D1 is the authoritative pinned copy and is correct; D2's bare token
list is the over-broad artifact. The class is titled "Interview/survey/study"
(REQ-CONTENT-003) and targets claims of research formality, not the curriculum
subject "social studies" or the study-guide deliverable. This does not change any
public contract: the D1 copy is unchanged, and the negative-test intent
(REQ-CONTENT-003: no claim of interviews/surveys/research studies) is fully
preserved. No implementation/code artifact changes; only the verification
matcher's scoping is clarified.
