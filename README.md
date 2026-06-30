# StorySongQwest Workspace

This workspace contains two active repositories plus workspace-level
documentation for routing work between them.

## Start Here

Read these documents in order:

1. `docs/architecture-map.md` - canonical workspace routing, ownership, and
   promotion process.
2. `docs/system-map.md` - compact dependency-flow summary.
3. `qwest-design-system/README.md` - Experience Lab setup, routes, and prototype
   status.
4. `story-song-qwest/README.md` - production runtime setup, build, test, bundle,
   and deployment commands.

## Workspace Structure

```text
StorySongQwest/
├── qwest-design-system/      # Experience Lab: UX prototypes and validation
├── story-song-qwest/         # Runtime: production app, platform, publishing
├── docs/                     # Workspace architecture, system maps, ADRs
└── README.md                 # Workspace entry point
```

## Current Status

- Current phase: Phase 5 - Runtime Promotion and Product Readiness.
- `qwest-design-system`: active Experience Lab.
- `story-song-qwest`: active production runtime development.
- StoryQwest reader prototypes and shared interaction patterns are the strongest
  promotion candidates.
- SongQwest has static Experience Lab evidence in `/listener` and
  `/labs/song-authoring`, but validation is pending. SongQwest runtime promotion
  remains blocked until accessibility review, audio abstraction, mixer
  contracts, timeline contracts, and promotion decisions are complete.

## Repository Roles

| Repository | Role |
| --- | --- |
| `qwest-design-system` | Prototype visual design, interaction models, reader/listener experiences, authoring concepts, accessibility states, and responsive behavior. |
| `story-song-qwest` | Own production runtime logic, canonical data contracts, Firebase, Cloudflare, publishing, tests, deployment, and runtime documentation. |
| `docs` | Own workspace-level architecture, feature routing, system maps, and workspace-wide ADRs. |

## Documentation Authority

`README.md` is only the workspace entry point and short status summary.
`docs/architecture-map.md` is the canonical source for repository ownership,
feature routing, promotion states, boundary rules, and documentation governance.
Supporting maps, roadmaps, milestones, audits, phase specs, and repo READMEs
must align with it.

Workspace-wide ADRs live in `docs/decisions/`. Repo-local ADRs may live under a
repository's own `docs/decisions/` only when the decision is specific to that
repository's implementation.

## Before Making Changes

1. Read `docs/architecture-map.md`.
2. Identify whether the task belongs in the Experience Lab, runtime, docs, or a
   sequenced cross-repository promotion path.
3. Check both repositories for existing prototypes, runtime implementations, and
   related documentation.
4. Preserve the distinction between prototype evidence, validated prototype,
   promoted runtime component, and production feature.
