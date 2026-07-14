# ADR-0006: Make The Qwest Medium The Product Authority For Phase 7

## Status

Accepted

## Date

2026-07-13

## Context

The workspace's architecture, runtimes, Creator tooling, platform boundaries,
preview environments, tests, and documentation advanced faster than the product
experience. The Creator/product Phase 7 specification consequently defined its
purpose as a marketplace and community platform, while the Experience Platform
roadmap independently defined Phase 7 as broad shared-service expansion.

The StoryQwest / SongQwest Product Vision Interview returned to the reason the
products exist. The interview established that StoryQwest and SongQwest are not
feature collections or commerce surfaces. They are expressions of a Qwest
medium designed around presence, attention, credible worlds, authentic agency,
world-specific expression, narrative music, and emotional continuity.

Continuing the existing Phase 7 roadmaps would make conventional software and
commercial infrastructure the next product objective before the medium itself
has been fully defined and realized.

## Decision

Adopt [`docs/product-vision.md`](../product-vision.md) as the canonical product
authority for what the Qwest medium should feel like and why it exists.

Rewrite Creator/product Phase 7 as **Build the Qwest Medium**, organized around:

1. The Qwest Medium.
2. Unified Creator Studio.
3. Reader & Listener Experience.
4. Platform Support.

Phase 7 milestones must trace directly to the product principles. When older
roadmap items conflict with those principles, the product vision wins.

Phase 7 proves those principles through a small number of complete,
production-path reference Qwests. It does not require exhaustive grammar,
Creator Studio, runtime, world-building, or Platform breadth before those
experiences work. Thin end-to-end evidence takes priority over broad partial
systems.

The Experience Platform retains the ownership boundaries accepted in ADR-0005,
but Platform work enters Phase 7 only when it is required to create, validate,
publish, deliver, resume, make accessible, or safely operate the Qwest medium.
Ownership does not itself justify implementation priority.

Marketplace, commerce, payouts, ratings, reviews, wishlists, following,
recommendations, generalized community systems, engagement optimization, and
broad shared-service expansion are deferred from Phase 7. They may be
reconsidered only after the medium's experience criteria and first-buy-in gate
are met, and only if they can still pass the canonical Product Decision Test.

## Alternatives Considered

### Continue The Marketplace And Community Phase

- Pros: Establishes monetization, creator payouts, discovery, and commercial
  distribution.
- Cons: Optimizes a commercial loop before the medium has a coherent experience;
  introduces ratings, follows, wishlists, conversion metrics, and completion
  pressure that can redirect attention from worlds to platform mechanics.
- Rejected: Commerce can distribute a medium, but it cannot define one.

### Continue Broad Shared Platform Services As The Next Phase

- Pros: Completes conventional application destinations and account services.
- Cons: Treats technical ownership and software completeness as product priority;
  risks building services that do not strengthen presence, creation, or the
  audience's relationship with a Qwest.
- Rejected: Platform support must be pulled by medium requirements.

### Preserve The Interview Only As Informal Context

- Pros: Avoids changing current documentation authority.
- Cons: Future contributors and agents would rediscover or override the product
  reasoning from technical roadmaps and feature lists.
- Rejected: The decision is too foundational to remain outside the workspace's
  durable documentation.

## Consequences

- `docs/product-vision.md` governs product intent; `docs/product-language.md`
  continues to govern terminology; `docs/architecture-map.md` continues to
  govern repository ownership and feature routing.
- Creator/product Phase 7 is no longer a marketplace or community phase.
- Experience Platform Phase 7 is subordinate Platform Support, not an independent
  mandate to build every planned shared service.
- Existing marketplace and broad Platform planning remain historical planning
  evidence, not active Phase 7 scope.
- Milestone acceptance must include experience evidence about presence,
  attention, restraint, authenticity, emotional continuity, and accessibility;
  feature completion alone is insufficient.
- At least one Story-first and one Music-first reference Qwest must traverse the
  production authoring, validation, preview, publishing, runtime, continuity,
  and accessibility path before Phase 7 can close.
- Marketplace work may be reconsidered only after direct product evidence shows
  that both reference Qwests feel meaningfully different from the media forms
  they transcend and creators can produce them without fighting technical
  machinery.
- New infrastructure requires an explicit trace to a product-vision principle
  and an active reference-Qwest need or release-safety obligation.
