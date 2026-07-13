# ADR-0005: Phase 6 Platform And Runtime Ownership

## Status

Accepted

## Date

2026-07-13

## Context

ADR-0001 established the multi-repository workspace and correctly kept
experimental work separate from production runtimes. ADR-0002 established the
prototype-promotion model. Their original wording predates the bounded
Experience Platform production-runtime integration completed through Platform
Phases 6A-6H.

The implemented architecture now includes a Platform-owned Runtime Host,
production StoryQwest and SongQwest providers, compatibility and registration
control planes, authentication and persistence ownership boundaries, bounded
runtime evidence, media/offline policy, and accessibility responsibility
negotiation. Both runtimes expose runtime-owned host adapters.

The workspace needs one durable ownership record that reflects this architecture
without rewriting the earlier ADRs or transferring runtime behavior to the
Platform.

## Decision

Preserve the multi-repository strategy and promotion model with this production
ownership split:

- `qwest-experience-platform` owns hosting, experience and provider registration,
  lifecycle coordination, compatibility policy, application identity and
  authentication boundary, persistence ownership/reconciliation boundary,
  Platform accessibility and focus boundary, media availability/download/offline
  policy, permitted runtime-evidence consumption, and shared Platform services.
- `story-song-qwest` owns StoryQwest and SongQwest rendering, runtime behavior,
  runtime state, runtime progress and protected-operation authorization,
  runtime media playback/presentation, runtime accessibility, runtime lifecycle
  implementation and teardown, canonical content/publishing, and the public
  runtime-host adapters.
- The Platform may consume only typed, bounded runtime evidence. It does not
  acquire authority to reproduce runtime behavior or mutate runtime-owned state.
- Creator Studio imports finalized media and authors canonical interactive Qwest
  content. SongQwest is not a DAW; recording, waveform editing, stem editing,
  mixing, and mastering are not part of the established architecture.

This decision narrows ADR-0001’s earlier phrase that assigned “platform
integration” and “persistence” wholesale to `story-song-qwest`. Runtime progress
persistence remains runtime-owned; Platform persistence policy and shared-service
data remain Platform-owned.

This decision also supersedes ADR-0002’s dated consequence that SongQwest runtime
promotion remained blocked. The promotion model remains accepted; the blocker
was subsequently resolved by the production Listener and hosted adapter work.

## Alternatives Considered

### Keep all production concerns in `story-song-qwest`

- Rejected: It contradicts the implemented Platform host, registration,
  compatibility, identity, persistence-policy, media, and accessibility
  boundaries and would blur shared Platform services with runtime behavior.

### Move runtime state and behavior into the Platform

- Rejected: Hosting a runtime does not transfer rendering, progress, playback,
  interaction, accessibility, or teardown authority.

### Rewrite ADR-0001 and ADR-0002 in place

- Rejected: Their historical context and original trade-offs remain useful.
  Later decisions should qualify earlier records rather than erase chronology.

## Consequences

- Canonical maps and repository READMEs must use the bounded ownership split.
- Phase 6A-6H records remain historical evidence and are not rewritten when
  later milestones change global project status.
- Roadmaps and readiness documents remain current-status sources and must not
  overstate production certification.
- Phase 7 may build shared Platform services without reopening runtime ownership
  unless new production evidence exposes a boundary defect.
- Physical-device validation, assistive-technology certification, durable offline
  infrastructure, production UI, Public Beta, deployment verification, and
  production launch remain separate future gates.

