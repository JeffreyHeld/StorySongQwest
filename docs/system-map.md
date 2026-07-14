# System Map - Supporting Dependency Graph

> **Documentation class: Canonical.** This is the compact view of long-lived
> workspace relationships, not a milestone-status ledger. See
> [Documentation Taxonomy](documentation-taxonomy.md).

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
Qwest Product Vision
purpose, principles, emotional standard
            |
            v
Phase scope and canonical contracts
            |
            v
qwest-experience-platform
Experience Lab + Platform boundaries
            |
            v
Promotion and integration evidence
            |
            v
story-song-qwest production runtimes
StoryQwest Reader + SongQwest Listener
            |
            v
Runtime-owned host adapters
            |
            v
Experience Platform Runtime Host
registration, lifecycle, identity and
persistence boundaries, media policy,
accessibility handoff
```

## Development Flow

1. **Qwest Product Vision**
   - Canonical purpose, experience principles, emotional standard, and Product
     Decision Test

2. **Phase Specs**
   - Bounded phase scope aligned to canonical product intent

3. **qwest-experience-platform**
   - Active Experience Lab
   - Bundle tooling
   - Shared platform models
   - Bundle Explorer and Bundle Inspector
   - Foundation Themes

Legacy `qwest-design-system` prototypes remain historical Experience Lab
evidence and reference material while migration proceeds.

4. **Static Evidence**
   - Reader
   - Listener
   - Authoring
   - Gallery
   - SongQwest listener/timeline evidence

5. **Validation / Contracts**
   - UX review
   - Accessibility review
   - Runtime and adapter contracts
   - Data contracts

6. **Promotion Candidate**
   - Validated evidence
   - Explicit promotion level
   - Blockers resolved or documented

7. **Runtime Planned**
   - Runtime milestone
   - Acceptance criteria
   - Verification steps

8. **Production Runtime**
   - StoryQwest runtime
   - SongQwest runtime

9. **Integrated**
   - Runtime implementation complete
   - Tests and docs complete
   - Prototype disposition recorded

10. **Published Bundle**
   - Portable Qwest output

11. **Reader / Listener Runtime**
    - StoryQwest reader experience
    - SongQwest listener experience

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
