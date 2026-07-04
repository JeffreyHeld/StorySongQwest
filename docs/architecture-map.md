# Architecture Map: Feature Ownership And Promotion

## Objective

This document is the canonical workspace architecture and feature ownership map
for StorySongQwest. It answers one question before any feature work begins:

> Where does this feature belong, and what must happen before it is production
> ready?

Use this document when a new idea, bug, feature, prototype, schema change,
runtime component, publishing change, or UX validation task enters the project.
It maps work to the correct repository, specification layer, validation step,
and promotion path.

This is the canonical architecture document for workspace-level ownership and
routing. It does not define production code by itself. It governs how supporting
system maps, runtime specs, promotion roadmaps, milestone plans, and phase specs
fit together.

It is also the canonical workspace process document. Durable guidance about
repository ownership, promotion routing, promotion states, boundary rules, and
documentation governance belongs here or in an accepted ADR, not in the root
README.

## Documentation Authority

Documentation authority flows in this order:

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

- `README.md` is the navigation entry point. It helps maintainers and agents find
  the right documents, but it does not supersede this architecture map.
- `docs/architecture-map.md` is canonical for workspace architecture, repository
  ownership, feature routing, and promotion ownership.
- `docs/product-language.md` is canonical for platform vocabulary, runtime
  vocabulary, Qwest mechanics, reserved terminology, naming principles, and the
  Explorer/Audience/Creator experience personas.
- `docs/system-map.md` is a compact supporting diagram and development-flow
  summary.
- Runtime specs, phase specs, promotion roadmaps, milestone plans, and promotion
  audits provide subordinate detail. If they conflict with this architecture map
  on ownership or routing, update the subordinate document or explicitly revise
  this map first.
- Workspace-wide ADRs live in `docs/decisions/`.
- Repository-local ADRs may live under a repository's own `docs/decisions/`
  only when the decision is specific to that repository's implementation and
  does not govern the whole workspace.

## Assumptions

- The workspace contains active runtime and platform repositories plus a legacy
  prototype repository:
  - `qwest-experience-platform`: the active Experience Lab and platform
    foundation for bundle tooling, shared platform models, Bundle Explorer,
    Bundle Inspector, Foundation Themes, and future platform experiments.
  - `story-song-qwest`: the production runtime, authoring runtime, publishing
    pipeline, Firebase integration, Cloudflare integration, tests, and platform
    architecture docs.
  - `qwest-design-system`: the legacy UI prototype repository containing
    historical Experience Lab work, reference implementations, and archived
    experiments.
- Product intent is captured in phase specifications under
  `story-song-qwest/docs`.
- Historical Experience Lab readiness is captured in
  `qwest-design-system/docs/experience-lab-promotion-audit.md`. Active
  Experience Lab readiness should be documented in `qwest-experience-platform`
  as migration work proceeds.
- Production promotion rules are captured in
  `story-song-qwest/docs/songqwest-storyqwest-phase-5-runtime-promotion-roadmap.md`.
- Execution sequencing is captured in
  `story-song-qwest/docs/songqwest-storyqwest-phase-5-runtime-milestones.md`.
- UI concepts should be proven in the Experience Lab before production runtime
  implementation unless the change is an urgent runtime defect or a purely
  backend/platform concern.
- Product terminology is governed by `docs/product-language.md`. Architecture
  docs may reference its terms, but should not redefine them.

## Master Dependency Graph

See `docs/system-map.md` for the compact dependency graph. The operational
version is:

```text
Phase Specs
  Product intent, canonical model, constraints, phase scope
        |
        v
qwest-experience-platform / Experience Lab
  Visual exploration, interaction prototypes, design tokens, gallery states
        |
        v
Prototype
  Reader, listener, authoring, media, reveal, discovery, choice, ambient,
  timeline, accessibility, responsive, and diagnostic experiments
        |
        v
Promotion Audit
  Ready / missing / duplicated / blocked / lab-only / candidate inventory
        |
        v
Runtime Promotion Roadmap
  Promotion level, ownership boundary, data contract, accessibility and
  performance responsibility
        |
        v
Runtime Milestone
  Ordered implementation work, acceptance criteria, verification commands,
  prototype disposition
        |
        v
story-song-qwest Runtime
  Production components, runtime engines, state integration, publishing,
  Firebase, Cloudflare, tests
        |
        v
Published Bundle
  Portable Qwest output and runtime-consumable indexes/assets
        |
        v
Reader / Listener Runtime
  StoryQwest reader experience, SongQwest listener experience, shared Qwest
  runtime behavior
```

## Repository Ownership Rules

| Work type                                  | Belongs in                                                                        | Why                                                                                                                       |
| ------------------------------------------ | --------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| New UI prototype                           | `qwest-experience-platform`                                                       | UX needs to be explored without production risk.                                                                          |
| New visual direction                       | `qwest-experience-platform`                                                       | Design language, motion, typography, and layout should be validated in the Experience Lab first.                          |
| New reader interaction concept             | `qwest-experience-platform` first                                                 | Reader behavior needs a prototype before promotion into production.                                                       |
| New listener or SongQwest timeline concept | `qwest-experience-platform` first                                                 | SongQwest timing, lyric, audio, and lane behavior needs a dedicated listener/timeline prototype before runtime promotion. |
| New authoring workflow                     | `qwest-experience-platform` first                                                 | Creator workflow should be proven as an authoring prototype before production integration.                                |
| New runtime component                      | `story-song-qwest`                                                                | Production components own state integration, error handling, accessibility, tests, and app boundaries.                    |
| New runtime engine                         | `story-song-qwest`                                                                | Engines are production architecture, not lab scaffolding.                                                                 |
| New Firebase behavior                      | `story-song-qwest`                                                                | Data persistence, auth, storage, security rules, and functions are production concerns.                                   |
| New Cloudflare behavior                    | `story-song-qwest`                                                                | Worker runtime, deployment, variables, bindings, and edge behavior are production concerns.                               |
| Publishing or bundle export change         | `story-song-qwest`                                                                | Bundles are production artifacts emitted by the runtime/publishing pipeline.                                              |
| Bundle fixture for visual QA               | `qwest-experience-platform` or exported from `story-song-qwest`                   | Lab fixtures validate presentation; production owns canonical export. Historical fixtures may remain in `qwest-design-system/public/bundles`. |
| Canonical content model change             | `story-song-qwest/docs` first, then runtime                                       | Schema and runtime architecture require explicit documentation before implementation.                                     |
| Platform architecture decision             | `story-song-qwest/docs`                                                           | Architecture decisions govern production behavior and should be documented near the runtime specs.                        |
| Promotion readiness assessment             | `qwest-experience-platform/docs`                                                  | Active audits record what the lab has proven and what remains missing. Historical audits remain in `qwest-design-system/docs`. |
| Promotion process rule                     | `story-song-qwest/docs/songqwest-storyqwest-phase-5-runtime-promotion-roadmap.md` | The roadmap defines how prototypes become runtime.                                                                        |
| Execution sequencing                       | `story-song-qwest/docs/songqwest-storyqwest-phase-5-runtime-milestones.md`        | Milestones define order, dependencies, acceptance criteria, and verification.                                             |
| Public beta release planning               | `story-song-qwest/docs`                                                           | Release notes, known issues, demo bundles, and beta gates are production launch artifacts.                                |

## Fast Routing Questions

Ask these questions in order.

1. **Is this changing product intent, phase scope, or canonical data shape?**
   - Yes: update `story-song-qwest/docs` first.
   - No: continue.

2. **Is this a user-facing UI, workflow, motion, layout, or interaction idea?**
   - Yes: start in `qwest-experience-platform`.
   - No: continue.

3. **Does the idea need UX validation before it is safe to ship?**
   - Yes: build or update an Experience Lab prototype.
   - No: continue.

4. **Is the prototype validated and production-relevant?**
   - Yes: record readiness in the promotion audit, then route through runtime
     promotion.
   - No: keep it in the lab or mark it missing/blocked.

5. **Does the work require production state, persistence, security, publishing,
   deployment, or runtime tests?**
   - Yes: implement in `story-song-qwest`.
   - No: keep it in `qwest-experience-platform` as lab work, or in
     `qwest-design-system` only when preserving historical reference material.

6. **Does the work cross repository boundaries?**
   - Yes: document the contract first, then implement sequentially:
     Experience Lab -> Promotion Audit -> Runtime Promotion -> Runtime
     Milestone -> Production Runtime.

## Feature List And Ownership

### Experience Platform And Experience Lab

| Feature area                 | Primary owner                                        | Required docs                    | Promotion gate                                                                                          |
| ---------------------------- | ---------------------------------------------------- | -------------------------------- | ------------------------------------------------------------------------------------------------------- |
| Design tokens                | `qwest-experience-platform` first, then `story-song-qwest` | Promotion audit, runtime roadmap | Tokens split from lab CSS and accepted as production primitives.                                        |
| Typography scale             | `qwest-experience-platform` first                    | Promotion audit                  | Reader/listener typography validated across desktop/mobile and accessibility modes.                     |
| Theme exploration            | `qwest-experience-platform`                          | Promotion audit                  | Foundation Themes are stable, documented, and not tied to lab-only layout styles.                       |
| Reader shell prototype       | `qwest-experience-platform`                          | Promotion audit                  | Validated reader shell can be promoted without importing lab routes.                                    |
| Listener shell prototype     | `qwest-experience-platform`                          | Promotion audit                  | Static legacy SongQwest listener evidence exists in `qwest-design-system`; validation and contract review gate promotion. |
| Component gallery            | `qwest-experience-platform`                          | Promotion audit                  | Examples identify canonical variants and lab-only variants.                                             |
| Visual regression fixtures   | `qwest-experience-platform`                          | Promotion audit                  | Fixture bundles cover reader, listener, media, reveal, discovery, choice, ambient, and timeline states. |
| Responsive layout prototypes | `qwest-experience-platform`                          | Promotion audit                  | Mobile, tablet, desktop, and ultra-wide states are validated before production.                         |
| Accessibility prototypes     | `qwest-experience-platform`                          | Promotion audit                  | Keyboard, focus, reduced-motion, and high-contrast behavior are proven before promotion.                |

### StoryQwest Reader

| Feature area             | Primary owner                                        | Required docs                                 | Promotion gate                                                                          |
| ------------------------ | ---------------------------------------------------- | --------------------------------------------- | --------------------------------------------------------------------------------------- |
| Reader runtime component | `story-song-qwest`                                   | Runtime milestone                             | Corresponding lab prototype exists and promotion level is explicit.                     |
| Reader surface           | `story-song-qwest`                                   | Runtime milestone                             | Typography, layout, media, and choice rendering are validated.                          |
| Choice cards             | `qwest-experience-platform` first, then `story-song-qwest` | Promotion audit, runtime milestone            | Base, locked, timed, disabled, selected, and script-driven states are covered.          |
| Journey Map UI           | `qwest-experience-platform` first, then `story-song-qwest` | Phase 4 spec, Phase 5 spec, runtime milestone | Generated from canonical graph model, not hand-authored display data.                   |
| Discovery surface        | `qwest-experience-platform` first, then `story-song-qwest` | Promotion audit, runtime milestone            | Canonical default presentation is selected from lab variants.                           |
| Reveal presentation      | `qwest-experience-platform` first, then `story-song-qwest` | Promotion audit, runtime milestone            | Click, hold, keyboard, reduced-motion, and fallback behavior are defined.               |
| Media moments            | `qwest-experience-platform` first, then `story-song-qwest` | Promotion audit, runtime milestone            | Overlay/card behavior includes focus return, captions, alt metadata, and asset loading. |
| Reader settings          | `qwest-experience-platform` first, then `story-song-qwest` | Promotion audit, runtime milestone            | Lab-only knobs are separated from supported user preferences.                           |
| Reader memory timeline   | `qwest-experience-platform` first, then `story-song-qwest` | Promotion audit, runtime milestone            | Static entries are replaced with runtime state adapter input.                           |

### SongQwest Listener

| Feature area                 | Primary owner                                        | Required docs                                                | Promotion gate                                                                                                          |
| ---------------------------- | ---------------------------------------------------- | ------------------------------------------------------------ | ----------------------------------------------------------------------------------------------------------------------- |
| SongQwest listener prototype | `qwest-experience-platform`                          | Promotion audit                                              | Static legacy evidence exists in `qwest-design-system`; validation, accessibility review, and promotion-level decisions remain required. |
| Performance timeline lanes   | `qwest-experience-platform` first, then `story-song-qwest` | SongQwest authoring spec, promotion audit, runtime milestone | Static legacy evidence exists in `qwest-design-system` `/listener` and `/labs/song-authoring`; lane taxonomy, timing ownership, and event contracts must be validated before runtime work. |
| Lyric presentation           | `qwest-experience-platform` first, then `story-song-qwest` | Promotion audit, runtime milestone                           | Active lyric, context, timing fallback, and accessibility behavior require fixture and accessibility validation before promotion. |
| Audio controls               | `qwest-experience-platform` first, then `story-song-qwest` | Promotion audit, runtime milestone                           | Static controls are evidence only; runtime promotion is blocked until the audio abstraction, autoplay handling, seek model, and accessibility review are complete. |
| Ambient mixer                | `story-song-qwest` after lab validation              | Runtime roadmap, runtime milestone                           | Runtime promotion is blocked until mixer contracts, audio-layer health, and metadata ownership are defined. |
| Stem crossfades              | `story-song-qwest`                                   | Architecture docs, runtime milestone                         | Requires audio engine abstraction before UI promotion.                                                                  |
| VU meters and faders         | `qwest-experience-platform` first                    | Promotion audit                                              | Prototype proves diagnostic utility before runtime integration.                                                         |
| Performance log              | `story-song-qwest` after diagnostic design           | Runtime milestone                                            | Diagnostic event and mixer contracts must exist before UI display. |
| Song Journey Map             | `qwest-experience-platform` first, then `story-song-qwest` | Phase 4 spec, Phase 5 spec, runtime milestone                | Generated from canonical graph/progression model and does not assume next/previous track navigation.                    |

### Creator Studio And Authoring

| Feature area                   | Primary owner                                        | Required docs                                     | Promotion gate                                                                |
| ------------------------------ | ---------------------------------------------------- | ------------------------------------------------- | ----------------------------------------------------------------------------- |
| StoryQwest authoring workflow  | `qwest-experience-platform` first                    | StoryQwest authoring design spec, promotion audit | Prototype covers write, structure, branch, enhance, polish, test, publish.    |
| SongQwest authoring workflow   | `qwest-experience-platform` first                    | SongQwest authoring design spec, promotion audit  | Static legacy timeline/lane prototype exists; validate interaction model, diagnostics, accessibility, and contract needs before production work. |
| Manuscript workspace           | `qwest-experience-platform` first, then `story-song-qwest` | Authoring design spec, runtime milestone          | Writing surface and structural editing behavior are validated.                |
| Performance timeline workspace | `qwest-experience-platform` first, then `story-song-qwest` | SongQwest design spec, runtime milestone          | Static legacy authoring evidence exists in `qwest-design-system` `/labs/song-authoring`; validation and contracts are still required. |
| Target selector                | `qwest-experience-platform` first                    | Promotion audit                                   | Real component exists beyond lab placeholder before production integration.   |
| Interaction command palette    | `qwest-experience-platform` first                    | Promotion audit                                   | Command language maps to canonical interaction taxonomy.                      |
| Script containers              | `qwest-experience-platform` first                    | Phase 4 spec, authoring design spec               | Data/schema contract exists before runtime implementation.                    |
| Preview sandbox                | `qwest-experience-platform` first, then `story-song-qwest` | Runtime milestone                                 | Production preview loads the same runtime package used by consumers.          |
| Validation overlay             | `story-song-qwest`                                   | Runtime milestone                                 | Validation rules are production-owned and performance-safe.                   |
| Publish readiness checklist    | `story-song-qwest`                                   | Runtime milestone, launch docs                    | Checklist maps to bundle validation, assets, paths, and deployment readiness. |

### Runtime Architecture And Platform

| Feature area               | Primary owner                                          | Required docs                                    | Promotion gate                                                                       |
| -------------------------- | ------------------------------------------------------ | ------------------------------------------------ | ------------------------------------------------------------------------------------ |
| Product language and terminology governance | Root `docs/product-language.md` | Architecture map, system map, promotion audit, runtime roadmap | Platform, runtime, and Qwest terminology remain distinct and reserved terms are not reused for navigation. |
| Canonical content model    | `story-song-qwest/docs`                                | Phase 4 spec, ADR if major                       | Schema impact and migration path are documented first.                               |
| Runtime state engine       | `story-song-qwest`                                     | Phase 4 spec, runtime roadmap, ADR if major      | Pure engine boundaries and tests exist before UI consumption.                        |
| Interaction taxonomy       | Shared concept, production-owned in `story-song-qwest` | Phase 4 spec, promotion audit, runtime milestone | StoryQwest and SongQwest variants are documented explicitly.                         |
| Atmosphere contract        | `story-song-qwest/docs` first                          | Phase 5 spec, runtime roadmap, promotion audit   | Scene/performance atmosphere metadata shape is stable.                               |
| Media asset resolver       | `story-song-qwest`                                     | Runtime milestone                                | Handles production bundle assets, missing assets, alt text, captions, and fallbacks. |
| Bundle export              | `story-song-qwest`                                     | README, runtime docs                             | Output is portable and accepted by Experience Platform tooling and runtime renderers. |
| Published bundle schema    | `story-song-qwest/docs`                                | Phase specs, bundle docs                         | Bundle format is documented and versioned.                                           |
| Firebase auth/data/storage | `story-song-qwest`                                     | Runtime docs, ADR if major                       | Security rules and emulator tests cover changes.                                     |
| Firebase Functions         | `story-song-qwest`                                     | Runtime docs                                     | Local emulator and deployed-function behavior are documented.                        |
| Cloudflare Worker runtime  | `story-song-qwest`                                     | Runtime docs, ADR if major                       | Variables, secrets, bindings, preview, deploy, and rollback are documented.          |
| Deployment pipeline        | `story-song-qwest`                                     | Runtime README, launch docs                      | Build, preview, upload, deploy, and environment ownership are explicit.              |
| Public beta release        | `story-song-qwest/docs`                                | Runtime milestone, release notes                 | Version, demo bundles, known issues, support path, and rollback plan exist.          |

### Documentation And Decisions

| Feature area            | Primary owner                                     | Required docs             | Promotion gate                                                              |
| ----------------------- | ------------------------------------------------- | ------------------------- | --------------------------------------------------------------------------- |
| Workspace orientation   | Root `README.md`, `docs/system-map.md`, this file | Architecture map          | New maintainers can identify owner and next document in under five minutes. |
| Product language        | Root `docs/product-language.md`                   | Product language          | Platform, runtime, Qwest, reserved terminology, naming principles, and experience personas have one source of truth. |
| Phase specs             | `story-song-qwest/docs`                           | Phase 0-7 specs           | Phase docs define intent and constraints before build plans.                |
| Promotion audit         | `qwest-experience-platform/docs`                   | Experience Lab audit      | Audit reflects current prototype readiness and gaps. Historical audits remain in `qwest-design-system/docs`. |
| Runtime roadmap         | `story-song-qwest/docs`                           | Runtime promotion roadmap | Roadmap defines stable promotion rules.                                     |
| Runtime milestones      | `story-song-qwest/docs`                           | Runtime milestone file    | Milestones define executable tasks and acceptance criteria.                 |
| Engineering standards   | Root `docs/engineering-standards.md`              | Standards doc             | Current performance, accessibility, testing, security, observability, and release verification expectations. |
| Workspace ADRs          | Root `docs/decisions`                             | ADR files                 | Required for expensive-to-reverse workspace-level architecture decisions.   |
| Repo-local ADRs         | `<repo>/docs/decisions` when needed               | ADR files                 | Use only for implementation decisions scoped to that repository.            |
| Changelog/release notes | `story-song-qwest/docs` recommended               | Release docs              | Required for shipped beta/public changes.                                   |

## Promotion Readiness States

Use these labels consistently in docs and planning.

| State                 | Meaning                                                               | Allowed next step                           |
| --------------------- | --------------------------------------------------------------------- | ------------------------------------------- |
| `Idea`                | Product or UX concept exists, but no prototype/spec decision yet.     | Route through this architecture map.        |
| `Specified`           | Phase spec or focused spec defines the requirement.                   | Build prototype or architecture plan.       |
| `Prototype`           | Experience Lab artifact exists.                                       | Validate UX and record in audit.            |
| `Validated`           | Prototype has acceptable UX, responsive, and accessibility direction. | Promotion audit can mark it as a candidate. |
| `Promotion Candidate` | Audit identifies readiness and production relevance.                  | Runtime roadmap selects promotion level.    |
| `Runtime Planned`     | Milestone defines implementation, acceptance, and verification.       | Implement in production runtime.            |
| `Integrated`          | Production runtime includes the promoted capability.                  | Mark prototype disposition and update docs. |
| `Archived`            | Prototype is kept as history but no longer drives production.         | No action unless revived by new spec.       |
| `Blocked`             | Missing dependency prevents promotion.                                | Document blocker and owner.                 |

## Promotion Levels

| Level                               | Use for                                                           | Typical examples                                                        | Required output                                                   |
| ----------------------------------- | ----------------------------------------------------------------- | ----------------------------------------------------------------------- | ----------------------------------------------------------------- |
| Level 1: Direct Component Promotion | Stable presentational primitives.                                 | Typography, spacing, simple cards, buttons, static states.              | Runtime component or token with tests and docs.                   |
| Level 2: Refactored Promotion       | Components with behavior or state boundaries.                     | Reader shell, choice list, media overlay, discovery panel.              | Production-owned component preserving validated UX.               |
| Level 3: Runtime Reimplementation   | Engines, orchestration, timing, persistence, or platform systems. | Performance timeline, audio runtime, state engine, publishing pipeline. | Architecture design, tests, runtime integration, migration notes. |

## Terminology Governance

Use `docs/product-language.md` as the authoritative vocabulary reference. This
architecture map governs where work belongs and how it is promoted; the product
language document governs what shared terms mean.

The workspace intentionally keeps three terminology layers separate:

1. **Platform** terms describe media-agnostic navigation and shared account
   experiences such as Home, Browse, Library, Search, Collections, Profile,
   Notifications, and Settings.
2. **Runtime** terms describe production systems, contracts, engines, state,
   validation, publishing, deployment, and implementation readiness.
3. **Qwest** terms describe interactive mechanics inside a Qwest, including
   Journal, Map, Cast, Reveal, Discovery, Timeline, Branch, and Decision.

This separation prevents collisions such as using Discover for platform
navigation while Discovery remains an in-Qwest mechanic. New platform navigation
should prefer Browse, Home, Library, Featured, Collections, Search, or
Recommendations.

Platform Experience Architecture is defined in `docs/product-language.md`. Use
that document for the Explorer/Audience/Creator planning matrix, Library as the
persistent content hub, future Studio planning, and exploratory Paths language.
Paths is not an approved runtime navigation term unless a future Experience Lab
validation and architecture decision promotes it.

## StoryQwest Flow

```text
StoryQwest phase specs
        |
        v
Experience Lab reader/authoring prototype
        |
        v
Promotion audit readiness
        |
        v
Runtime promotion level selected
        |
        v
Runtime milestone scheduled
        |
        v
story-song-qwest production implementation
        |
        v
Published bundle
        |
        v
StoryQwest reader runtime
```

StoryQwest production work should not start from gallery code directly. It
should start from the validated behavior, documented contract, selected
promotion level, and runtime milestone.

## SongQwest Flow

```text
SongQwest phase specs
        |
        v
Experience Lab static listener/timeline evidence
        |
        v
SongQwest lane, accessibility, audio, mixer, and contract validation
        |
        v
Promotion audit readiness
        |
        v
Runtime promotion level selected
        |
        v
Runtime milestone scheduled
        |
        v
story-song-qwest SongQwest runtime implementation
        |
        v
Published bundle
        |
        v
SongQwest listener runtime
```

SongQwest runtime work must not treat static Experience Lab evidence as
runtime-ready. `/listener` and `/labs/song-authoring` currently provide
prototype evidence for listener hierarchy, lyric states, timeline lanes, mobile
layouts, audio fallback/accessibility states, authoring timeline concepts, and
mixer diagnostics. Runtime promotion remains blocked until accessibility review,
the audio runtime abstraction, mixer contracts, timeline contracts, and
component-level promotion decisions are complete.

## Boundary Rules

### Always

- Start by identifying whether the task is product intent, UX validation,
  runtime implementation, platform architecture, or release work.
- Check both repositories before making architectural decisions.
- Keep prototype code and production runtime code separated.
- Keep platform vocabulary, runtime vocabulary, and Qwest mechanics distinct by
  following `docs/product-language.md`.
- Record promotion readiness in the promotion audit before scheduling runtime
  promotion.
- Define data contracts before production runtime code depends on them.
- Update the relevant docs when a feature changes ownership, status, or scope.
- Verify accessibility and reduced-motion behavior for promoted UI.

### Ask First

- Changing canonical content model fields.
- Adding a new dependency to either repository.
- Changing Firebase data shape, security rules, storage layout, or Functions
  behavior.
- Changing Cloudflare deployment architecture, bindings, or environment
  variable ownership.
- Moving code between repositories.
- Treating a placeholder prototype as production-ready.
- Skipping the Experience Lab for a user-facing UI feature.

### Never

- Copy gallery or lab scaffolding directly into production runtime as-is.
- Put Firebase, Cloudflare, auth, persistence, or production state logic in the
  Experience Lab or legacy prototype repositories.
- Implement new production UI first when no prototype exists, unless explicitly
  approved as an urgent runtime fix.
- Treat static fixture data as proof that runtime state integration is ready.
- Mark a prototype `Integrated` unless production runtime, tests, docs, and
  milestone evidence are complete.
- Let SongQwest runtime work proceed as if reader prototypes fully validate
  audio/timeline/listener behavior.
- Treat SongQwest static listener or authoring evidence as a runtime-ready
  component before validation and contracts are recorded.

## Commands

Use repository-local commands when validating the docs or the systems described
by this map.

### Workspace Documentation Checks

```sh
rg -n "architecture-map|system-map|Promotion Audit|Runtime Promotion|Experience Lab" README.md docs story-song-qwest/docs qwest-experience-platform qwest-design-system/docs
```

### qwest-experience-platform

```sh
cd qwest-experience-platform
npm install
npm run dev
npm run build
```

### qwest-design-system Historical Reference

```sh
cd qwest-design-system
npm install
npm run dev
npm run build
npm run preview
npm run scan-bundles
```

### story-song-qwest

```sh
cd story-song-qwest
npm install
npm run dev
npm run build
npm run typecheck
npm test
npm run test:e2e
npm run dev:firebase
npm run test:emulators
npm run preview
npm run deploy
```

## Success Criteria

This architecture map is useful when:

- A maintainer can route a new feature to the correct repository without
  reading every phase spec first.
- A user-facing UI request clearly starts in the Experience Lab unless it is an
  approved runtime exception.
- A production runtime request clearly routes to `story-song-qwest`.
- A platform architecture request clearly routes to `story-song-qwest/docs`
  before implementation.
- A UX validation request clearly routes to `qwest-experience-platform`.
- A production-readiness request clearly routes through the promotion audit,
  runtime roadmap, and runtime milestones.
- SongQwest-specific work visibly depends on a real listener/timeline prototype.
- SongQwest-specific work clearly distinguishes static evidence from validated
  prototype, promotion candidate, and runtime-ready component.
- Promotion status labels make it clear whether a feature is an idea,
  prototype, candidate, planned runtime work, integrated capability, archived
  reference, or blocked dependency.

## Open Questions

1. Should the design system be formally React-capable, framework-neutral, or
   dual-mode?
2. What is the canonical status source: Experience Lab metadata, the promotion
   audit, runtime milestones, or a future shared tracking file?
3. What minimum validation is required before a prototype can be marked
   `Validated`?
4. What is the first required SongQwest deliverable after static
   listener/timeline evidence: accessibility review, audio abstraction, mixer
   contract, timeline contract, or promotion-level decision record?
