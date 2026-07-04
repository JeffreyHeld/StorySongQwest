# StorySongQwest Workspace

This workspace contains the root workspace/docs repository plus product and
prototype repositories for the Qwest platform.

## Start Here

Read these documents in order:

1. `docs/architecture-map.md` - canonical workspace routing, ownership, and
   promotion process.
2. `docs/product-language.md` - canonical platform, runtime, Qwest, reserved
   terminology, and experience persona vocabulary.
3. `docs/system-map.md` - compact dependency-flow summary.
4. `qwest-experience-platform/README.md` - active Experience Lab setup, bundle
   tooling, Bundle Explorer, Bundle Inspector, and foundation work.
5. `qwest-design-system/README.md` - legacy prototype reference and historical
   Experience Lab status.
6. `story-song-qwest/README.md` - production runtime setup, build, test, bundle,
   and deployment commands.

## Workspace Structure

```text
StorySongQwest/
├── qwest-experience-platform/ # Git repo: active Experience Lab and platform foundation
├── qwest-design-system/       # Git repo: legacy UI prototype reference
├── story-song-qwest/         # Git repo: product runtime, platform, publishing
├── docs/                     # Root Git repo: workspace maps, standards, ADRs
└── README.md                 # Workspace entry point
```

## Current Status

- Current phase: Phase 5 - Runtime Promotion and Product Readiness.
- `qwest-experience-platform`: active Experience Lab and platform foundation.
- `qwest-design-system`: legacy UI prototype repository and historical
  Experience Lab reference.
- `story-song-qwest`: active production runtime development.
- StoryQwest reader prototypes and shared interaction patterns are the strongest
  promotion candidates.
- SongQwest has static historical Experience Lab evidence in the legacy
  `qwest-design-system` `/listener` and `/labs/song-authoring` areas, but
  validation is pending. SongQwest runtime promotion remains blocked until
  accessibility review, audio abstraction, mixer contracts, timeline contracts,
  and promotion decisions are complete.

## Repository Roles

| Repository | Role |
| --- | --- |
| Root workspace/docs repo | Own workspace-level architecture, feature routing, system maps, engineering standards, and workspace-wide ADRs. |
| `qwest-experience-platform` | Own the active Experience Lab, platform foundation work, bundle tooling, shared platform models, Bundle Explorer, Bundle Inspector, Foundation Themes, and future platform experiments. |
| `qwest-design-system` | Preserve legacy UI prototypes, historical Experience Lab work, reference implementations, and archived experiments. |
| `story-song-qwest` | Own production runtime logic, canonical data contracts, Firebase, Cloudflare, publishing, tests, deployment, and runtime documentation. |

## Documentation Authority

`README.md` is only the workspace entry point and short status summary.
`docs/architecture-map.md` is the canonical source for repository ownership,
feature routing, promotion states, boundary rules, and documentation governance.
`docs/product-language.md` is the canonical source for platform, runtime, Qwest,
reserved terminology, and Explorer/Audience/Creator experience personas.
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
