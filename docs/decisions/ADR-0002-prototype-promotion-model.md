# ADR-0002: Prototype Promotion Model

## Status

Accepted

## Date

2026-06-30

## Context

The Experience Lab proves interaction, rhythm, layout, accessibility direction, motion language, and visual hierarchy quickly. The production runtime must make validated behavior durable, testable, accessible, performant, and safe at scale.

Directly copying prototype code into production would preserve lab scaffolding, mock state, temporary controls, inline fixtures, and route-specific assumptions. Conversely, ignoring prototypes would waste validated UX work and recreate decisions.

The workspace needs an explicit model for how prototypes become runtime features.

## Decision

Adopt a prototype-to-production promotion model:

```text
Experience Lab
  Prototype and validate experience
      ↓
Promotion Audit
  Record readiness, gaps, blockers, and candidate status
      ↓
Runtime Promotion Roadmap
  Select promotion level and architecture path
      ↓
Runtime Milestone
  Define implementation scope, acceptance criteria, and verification
      ↓
Production Runtime
  Implement with runtime-owned state, data, accessibility, tests, and docs
```

Promotion is an engineering decision, not a copy operation. A prototype can preserve durable intent, interaction behavior, visual hierarchy, component anatomy, and data contract concepts, but production must own runtime boundaries, state integration, validation, tests, accessibility, performance, and error handling.

Promotion levels are:

- Level 1: Direct Component Promotion for stable presentational primitives.
- Level 2: Refactored Promotion for components with behavior, composition, or state boundaries.
- Level 3: Runtime Reimplementation for engines, orchestration, timing, persistence, or platform systems.

## Alternatives Considered

### Copy prototype code directly into production

- Pros: Fast initial implementation.
- Cons: Imports lab assumptions, mock state, and unstable APIs into production.
- Rejected: It violates the workspace boundary and creates long-term runtime risk.

### Treat prototypes as disposable sketches only

- Pros: Maximum freedom for production architecture.
- Cons: Loses validated UX, accessibility findings, and interaction decisions.
- Rejected: Prototypes are reference specifications, not throwaway sketches.

### Promote every prototype the same way

- Pros: Simpler process.
- Cons: Presentational tokens, UI shells, interaction layers, and runtime engines have different risk profiles.
- Rejected: Promotion levels are needed to scale review effort to risk.

## Consequences

- No production UI feature should bypass the Experience Lab unless explicitly approved as an urgent runtime fix or purely backend/platform concern.
- The promotion audit must distinguish prototype evidence from runtime readiness.
- Runtime milestones must record promotion level, ownership, acceptance criteria, and verification.
- SongQwest runtime promotion remains blocked until listener/timeline evidence is validated and mapped to runtime contracts.
- Prototype disposition must be recorded as promoted, superseded, archived, or retained for further exploration.

## Later Decisions

[ADR-0005](ADR-0005-phase-6-platform-runtime-ownership.md) preserves the
promotion model and records that the dated SongQwest promotion blocker in the
consequences above was subsequently resolved by the production Listener and
runtime-host adapter work.
