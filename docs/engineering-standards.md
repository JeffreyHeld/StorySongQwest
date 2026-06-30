# Engineering Standards

This document is the single workspace-level reference for production engineering
standards that apply when Experience Lab evidence is promoted into runtime work.
It defines current expectations; it does not mark any SongQwest validation,
contract, or runtime implementation complete.

## Scope

These standards apply to production runtime work in `story-song-qwest` and to
promotion decisions that move evidence from `qwest-design-system` toward
runtime implementation. Lab-only prototypes may reference these standards, but
they are not considered production-ready until the relevant runtime milestone
verifies them.

## Performance

- Define performance acceptance criteria in the runtime milestone before
  implementation starts.
- Verify route load, interaction responsiveness, media loading, and layout shift
  for promoted reader/listener surfaces.
- Keep gallery, import, diagnostic, and lab-only code out of production bundles.
- Record any accepted performance tradeoff in the relevant milestone or launch
  notes.

## Accessibility

- Verify keyboard navigation, focus order, focus return, escape behavior,
  reduced-motion behavior, high contrast, labels, captions, transcripts, and
  fallback copy for promoted UI.
- Do not promote SongQwest audio, lyric, transport, timeline, mixer, or
  diagnostic surfaces until accessibility review is recorded.
- Provide non-motion and non-audio fallbacks where runtime behavior depends on
  animation, audio playback, captions, or transcript availability.

## Testing

- Match test depth to promotion risk:
  - Level 1: component, snapshot, and accessibility checks for presentational
    primitives.
  - Level 2: integration tests for state boundaries, focus behavior, fallbacks,
    and responsive behavior.
  - Level 3: architecture tests, contract tests, engine tests, integration tests,
    and end-to-end coverage for runtime orchestration.
- Use repository-local commands documented in the relevant README or milestone.
- Do not mark a prototype `Integrated` until runtime tests and documentation are
  complete.

## Security

- Keep Firebase, Cloudflare, auth, persistence, secrets, and production state
  logic out of `qwest-design-system`.
- Document schema, security rule, storage layout, function, Worker binding, and
  environment-variable changes before implementation.
- Never commit secrets. Runtime secrets belong in the deployment platform's
  variables/secrets scope.

## Observability

- Define runtime logging, diagnostics, and recovery ownership for promoted
  engines and media/audio systems.
- SongQwest mixer diagnostics and performance logs require explicit diagnostic
  event contracts before runtime work begins.
- Production telemetry must avoid leaking sensitive content, credentials, or
  private user data.

## Release Verification

- Runtime milestones must list acceptance criteria and verification commands.
- Launch readiness must include build, typecheck, relevant tests, deployment
  preview, accessibility review, known issues, and rollback notes.
- Public beta release planning must identify version, demo bundles, support path,
  known issues, and rollback plan.

## Current Open Standards Work

- Define the repeatable accessibility verification harness.
- Define visual regression coverage for reader themes, mobile surfaces, overlays,
  ambient effects, typography, choices, reveals, discoveries, and timeline
  states.
- Define SongQwest audio runtime, timeline, mixer, diagnostic, and accessibility
  contract checks before M5.3 can be unblocked.
