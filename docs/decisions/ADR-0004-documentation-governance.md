# ADR-0004: Documentation Governance

## Status

Accepted

## Date

2026-06-30

## Context

The workspace contains root documentation, workspace maps, repository-specific READMEs, phase specifications, promotion roadmaps, milestone plans, audits, and future ADRs.

Without an explicit authority hierarchy, documents can drift or compete:

- README can become both navigation and architecture.
- `docs/system-map.md` can be mistaken for the canonical architecture source.
- Runtime specs, promotion roadmaps, phase specs, and audits can contradict workspace ownership rules.

The project needs clear documentation ownership so humans and agents know which document governs navigation, architecture, and supporting detail.

## Decision

Adopt this documentation authority hierarchy:

```text
README.md
  Navigation entry point
      ↓
docs/architecture-map.md
  Canonical workspace architecture and feature ownership
      ↓
System maps, runtime specs, promotion roadmaps, and phase specs
  Supporting detail, execution plans, and subordinate specifications
```

`README.md` helps maintainers and agents find the right documents. It does not supersede the architecture map.

`docs/architecture-map.md` is canonical for workspace architecture, repository ownership, feature routing, and promotion ownership.

`docs/system-map.md` is a compact supporting dependency graph.

Runtime specs, phase specs, promotion roadmaps, milestone plans, audits, and repository READMEs provide subordinate detail. If they conflict with the architecture map on ownership or routing, update the subordinate document or explicitly revise the architecture map first.

ADRs live in `docs/decisions/` at the workspace root for workspace-level decisions.

## Alternatives Considered

### Treat README as the single source of truth

- Pros: Simple onboarding path.
- Cons: README becomes too large and mixes navigation, architecture, and execution detail.
- Rejected: README should remain the navigation entry point.

### Treat system-map as canonical

- Pros: Easy visual flow.
- Cons: A compact diagram cannot carry full ownership rules, exceptions, or promotion routing.
- Rejected: System map is supporting documentation.

### Keep ADRs inside `story-song-qwest/docs/decisions`

- Pros: Keeps production architecture decisions near runtime implementation.
- Cons: Workspace-level decisions govern both repositories and should not appear owned only by the runtime repo.
- Rejected for workspace-level ADRs: Root `docs/decisions/` is the correct home.

## Consequences

- Future workspace-level decisions should be captured in root `docs/decisions/`.
- README should stay short and navigational.
- Architecture-map changes are required before subordinate documents change ownership or routing rules.
- System maps, runtime specs, promotion roadmaps, phase specs, milestones, audits, and repository READMEs must align with the architecture map.
- Historical decisions should be superseded by new ADRs rather than deleted.

