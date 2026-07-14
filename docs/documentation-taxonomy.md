# Documentation Taxonomy

This taxonomy keeps long-lived architecture, current project status, and dated
implementation evidence from competing with one another.

## Canonical

Canonical documents contain long-lived truths: ownership, architectural
boundaries, terminology, governance, and accepted decisions. They change slowly
and only when the underlying decision changes.

Examples:

- `docs/architecture-map.md`
- `docs/product-vision.md`
- `docs/system-map.md`
- `docs/product-language.md`
- accepted ADRs in `docs/decisions/`

When a canonical decision changes, update the canonical map and write or
supersede an ADR. Do not use a roadmap or milestone record to silently redefine
ownership.

Canonical authority is scoped by subject:

- `docs/architecture-map.md` governs workspace ownership and feature routing.
- `docs/product-vision.md` governs product purpose, experience principles, and
  product-priority decisions.
- `docs/product-language.md` governs terminology and naming.

## Status

Status documents describe the current project state, next work, open gates, and
release posture. They evolve as milestones close.

Examples:

- `qwest-experience-platform/docs/roadmap.md`
- `qwest-experience-platform/docs/production-readiness.md`
- `story-song-qwest/docs/launch-readiness.md`
- repository README current-status sections

Status documents may summarize evidence, but should link to the historical
record rather than duplicate detailed test narratives.

## Historical Evidence

Historical evidence records exactly what was known, implemented, tested, and
deferred at a point in time. Later work may supersede its project-status
statements, but not the evidence itself.

Examples:

- Phase 6A through Phase 6H integration records
- dated closeout findings
- milestone verification sections and exact command results
- superseded or deprecated ADRs

Use a banner when a historical record contains a status statement that was true
at closeout but is no longer globally current:

> **Historical milestone record.** Later milestones supersede project status but
> not the evidence contained here.

When useful, name the later state explicitly:

> **Historical note:** This document reflects the state at the conclusion of
> Phase 6C. Phase 6 was subsequently completed through Phase 6H.

Do not rewrite statements such as “does not complete Phase 6” when they correctly
describe the milestone at the time. Add context above them.

## Phase Names

The workspace has more than one roadmap vocabulary. Always qualify a phase when
the surrounding document does not make its scope obvious:

- **Experience Platform Phase 6** — production runtime integration architecture.
- **Runtime milestone M5.x** — StoryQwest/SongQwest production-runtime promotion
  and launch readiness.
- **Creator/product Phase 6** — the separately scoped Creator Studio evolution
  specification.

Completion in one track does not imply completion in another.
