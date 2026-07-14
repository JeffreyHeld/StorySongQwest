# StorySongQwest Workspace

This workspace contains the root workspace/docs repository plus product and
prototype repositories for the Qwest platform.

## Start Here

Read these documents in order:

1. `docs/architecture-map.md` - canonical workspace routing, ownership, and
   promotion process.
2. `docs/product-vision.md` - canonical product purpose, experience principles,
   emotional standard, and Product Decision Test.
3. `docs/product-language.md` - canonical platform, runtime, Qwest, reserved
   terminology, and experience persona vocabulary.
4. `docs/qwest-content-model/overview.md` - canonical published Qwest content
   model and links to its manuscript, section, runtime graph, media,
   interaction, world, map, journal, progress, and validation contracts.
5. `docs/documentation-taxonomy.md` - canonical, status, and historical-evidence
   document roles.
6. `docs/system-map.md` - compact dependency-flow summary.
7. `qwest-experience-platform/README.md` - active Experience Lab and Platform
   setup, bundle tooling, Bundle Explorer, Bundle Inspector, and foundation work.
8. `qwest-design-system/README.md` - legacy prototype reference and historical
   Experience Lab status.
9. `story-song-qwest/README.md` - production runtime setup, build, test, bundle,
   and deployment commands.

## Workspace Structure

```text
StorySongQwest/
├── qwest-experience-platform/ # Git repo: active Experience Lab and platform foundation
├── qwest-design-system/       # Git repo: legacy UI prototype reference
├── story-song-qwest/         # Git repo: product runtime, platform, publishing
├── docs/                     # Root Git repo: workspace maps, content model, standards, ADRs
└── README.md                 # Workspace entry point
```

## Current Status

SongQwest is an import-first interactive storytelling product, not a DAW.
Artists produce music outside SongQwest, then Creator Studio imports finalized
audio, organizes scenes and sections, attaches lyrics and interactive content,
adds optional branches, previews and validates the production runtime, and
publishes. See `docs/product-language.md` for the canonical workflow.

- Experience Platform Phase 6 production runtime integration architecture is
  complete within its bounded 6A-6H ownership scope. Creator/product Phase 7 is
  now **Build the Qwest Medium**. Platform Phase 7 work is limited to support
  directly required by that product vision. Phase 7 is proven through one
  Story-first and one Music-first production-path reference Qwest, using complete
  vertical slices rather than broad partial systems.
- Runtime milestone M5.5 Launch Readiness is Testing. M5.6 Public Beta,
  deployment validation, physical-device QA, assistive-technology certification,
  and production launch remain incomplete.
- `qwest-experience-platform`: active Experience Lab, Runtime Host, registration,
  compatibility, identity/persistence boundaries, runtime evidence, media/offline
  policy, and accessibility-responsibility integration.
- `qwest-design-system`: legacy UI prototype repository and historical
  Experience Lab reference.
- `story-song-qwest`: active production runtime development.
- StoryQwest and SongQwest production runtimes expose runtime-owned host adapters
  consumed by the Experience Platform through one generic provider/host boundary.
- Legacy SongQwest listener, timeline, mixer, fader, and stem concepts remain
  historical evidence only. The canonical Creator workflow imports finalized
  audio and does not add DAW or music-production scope.

## Repository Roles

| Repository | Role |
| --- | --- |
| Root workspace/docs repo | Own workspace-level architecture, feature routing, system maps, engineering standards, and workspace-wide ADRs. |
| `qwest-experience-platform` | Own the active Experience Lab plus Platform hosting, registration, lifecycle, compatibility, authentication and persistence boundaries, shared-service architecture, media availability/download policy, and shell accessibility. |
| `qwest-design-system` | Preserve legacy UI prototypes, historical Experience Lab work, reference implementations, and archived experiments. |
| `story-song-qwest` | Own StoryQwest/SongQwest rendering, runtime behavior and state, runtime progress persistence, runtime media behavior, runtime accessibility, canonical content/publishing, Firebase-backed protected operations, Cloudflare deployment, tests, and runtime documentation. |

## Documentation Authority

`README.md` is only the workspace entry point and short status summary.
`docs/architecture-map.md` is the canonical source for repository ownership,
feature routing, promotion states, boundary rules, and documentation governance.
`docs/product-vision.md` is the canonical source for product purpose,
experience principles, emotional standards, and the Product Decision Test.
`docs/product-language.md` is the canonical source for platform, runtime, Qwest,
reserved terminology, and Explorer/Audience/Creator experience personas.
`docs/qwest-content-model/` is the canonical source for the published Qwest
content model, manuscript/runtime graph relationship, sections, media,
interactions, world data, maps, journal knowledge, progress, and validation
contracts.
`docs/documentation-taxonomy.md` defines which documents are canonical, current
status, or historical evidence.
[ADR-0005](docs/decisions/ADR-0005-phase-6-platform-runtime-ownership.md)
records the implemented Experience Platform/runtime ownership split.
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
