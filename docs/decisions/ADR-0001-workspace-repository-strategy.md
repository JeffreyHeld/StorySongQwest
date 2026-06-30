# ADR-0001: Workspace Repository Strategy

## Status

Accepted

## Date

2026-06-30

## Context

StorySongQwest contains two active repositories inside one workspace:

- `qwest-design-system`: the Experience Lab for UX prototypes, visual language, design tokens, galleries, and validation.
- `story-song-qwest`: the production runtime for application logic, publishing, Firebase, Cloudflare, tests, deployment, and runtime architecture.

The project needs fast UX iteration without allowing experimental code to leak into production runtime systems. It also needs production code to stay stable, testable, secure, and consistent with the canonical Qwest runtime model.

StoryQwest and SongQwest share the same underlying runtime architecture wherever practical. Differences should primarily exist in authoring workflows, experience surfaces, and media pipelines.

## Decision

Keep the workspace as a two-repository structure:

- `qwest-design-system` owns prototype exploration and experience validation.
- `story-song-qwest` owns production runtime implementation, platform integration, persistence, deployment, and runtime tests.
- Workspace-level docs define how work moves between the repositories.

The repositories may exchange artifacts and evidence, but they must not directly share runtime code or cross-import implementation modules. Production work should inspect Experience Lab prototypes as reference behavior, then implement production-owned boundaries in `story-song-qwest`.

## Alternatives Considered

### Single repository for prototype and runtime

- Pros: Simpler local setup and easier file discovery.
- Cons: Increases risk of experimental UI, mock state, and lab scaffolding entering production paths.
- Rejected: The project needs a stronger boundary between exploration and runtime stability.

### Fully separate workspaces

- Pros: Strongest isolation between lab and runtime.
- Cons: Makes cross-repository architectural review, bundle fixture exchange, and agent onboarding harder.
- Rejected: The current workspace benefits from side-by-side inspection while preserving repository isolation.

### Shared component package immediately

- Pros: Could reduce duplication once stable primitives emerge.
- Cons: Premature package boundaries could freeze prototype choices before validation.
- Rejected for now: Shared packages should emerge only after promotion decisions define stable runtime contracts.

## Consequences

- Engineers and agents must inspect both repositories before making architectural decisions.
- UI and experience ideas start in `qwest-design-system` unless they are urgent runtime fixes or backend/platform concerns.
- Production implementation lives in `story-song-qwest`.
- Promotion requires documented evidence and runtime-owned contracts.
- Some duplication is expected during exploration; it becomes a problem only when it bypasses promotion review or persists after production integration.

