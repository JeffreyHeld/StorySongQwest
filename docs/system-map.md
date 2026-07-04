# System Map - Supporting Dependency Graph

This document is a compact supporting diagram for the StorySongQwest workspace.
It is subordinate to `docs/architecture-map.md`, which is the canonical source
for workspace architecture, repository ownership, feature routing, and promotion
ownership.

Terminology used by this map follows `docs/product-language.md`, the canonical
source for platform, runtime, Qwest, reserved terminology, and experience
personas.

Use this system map for a fast visual summary of the dependency flow. If this
document conflicts with `docs/architecture-map.md`, update this document or
revise the architecture map first.

```text
Phase Specs
Phase 0-7 product and platform intent
            |
            v
qwest-experience-platform
Experience Lab
            |
            v
Static Evidence
reader, listener, authoring,
gallery, SongQwest listener/timeline
            |
            v
Validation / Contracts
UX review, accessibility review,
audio abstraction, mixer contracts,
timeline contracts, data contracts
            |
            v
Promotion Candidate
validated evidence with explicit
promotion level and blockers resolved
            |
            v
Runtime Planned
runtime milestone, acceptance criteria,
verification, prototype disposition
            |
            v
story-song-qwest Runtime
production implementation with state,
tests, accessibility, deployment docs
            |
            v
Integrated
production runtime, docs, tests, and
prototype disposition complete
```

## Development Flow

1. **Phase Specs**
   - Phase 0–7 product and platform intent

2. **qwest-experience-platform**
   - Active Experience Lab
   - Bundle tooling
   - Shared platform models
   - Bundle Explorer and Bundle Inspector
   - Foundation Themes

Legacy `qwest-design-system` prototypes remain historical Experience Lab
evidence and reference material while migration proceeds.

3. **Static Evidence**
   - Reader
   - Listener
   - Authoring
   - Gallery
   - SongQwest listener/timeline evidence

4. **Validation / Contracts**
   - UX review
   - Accessibility review
   - Audio abstraction
   - Mixer contracts
   - Timeline contracts
   - Data contracts

5. **Promotion Candidate**
   - Validated evidence
   - Explicit promotion level
   - Blockers resolved or documented

6. **Runtime Planned**
   - Runtime milestone
   - Acceptance criteria
   - Verification steps

7. **Production Runtime**
   - StoryQwest runtime
   - SongQwest runtime only after validation/contracts

8. **Integrated**
   - Runtime implementation complete
   - Tests and docs complete
   - Prototype disposition recorded

9. **Published Bundle**
   - Portable Qwest output

10. **Reader / Listener Runtime**
    - StoryQwest reader experience
    - SongQwest listener experience only after integration

## Terminology Layers

Use `docs/product-language.md` as the source of truth for definitions. This map
keeps three terminology layers separate:

1. **Platform** - Media-agnostic navigation and account experiences such as
   Home, Browse, Library, Search, Collections, Profile, Notifications, and
   Settings.
2. **Runtime** - Production systems that render, validate, publish, persist,
   synchronize, and deploy Qwest experiences.
3. **Qwest** - Interactive content mechanics inside StoryQwest, SongQwest, and
   future Qwest formats, including Qwest, Journal, Map, Cast, Reveal,
   Discovery, Timeline, Branch, and Decision.

The layers remain separate so platform navigation does not collide with Qwest
mechanics and runtime architecture does not inherit Experience Lab or product
navigation terminology accidentally.

Platform Experience Architecture, including the Explorer/Audience/Creator
planning matrix, Library as the persistent content hub, future Studio planning,
and exploratory Paths concept, is defined in `docs/product-language.md`.
