# Product Language

## Product Language Overview

This document is the canonical vocabulary reference for StoryQwest, SongQwest,
the Experience Lab, the production runtime, specifications, and future planning
docs.

Use it to keep platform navigation, runtime systems, and Qwest mechanics
distinct. If another document needs a term definition, link here instead of
redefining the term locally.

Product language is organized into three layers:

- **Platform:** Media-agnostic product surfaces that help people find, manage,
  create, and return to experiences.
- **Runtime:** The production systems that render, persist, publish, validate,
  and synchronize experiences.
- **Qwest:** Interactive content mechanics that exist inside StoryQwest,
  SongQwest, and future Qwest media forms.

## Related Documents

- `docs/architecture-map.md` governs workspace architecture, repository
  ownership, feature routing, and promotion ownership.
- `docs/system-map.md` summarizes the Platform, Runtime, and Qwest terminology
  layers in the workspace dependency flow.
- `qwest-design-system/docs/experience-lab-promotion-audit.md` records
  Experience Lab planning status and foundational shared experience candidates.
- `story-song-qwest/docs/songqwest-storyqwest-phase-5-runtime-promotion-roadmap.md`
  governs how validated Experience Lab work becomes production runtime.

## Platform Vocabulary

Platform terms describe navigation and durable product surfaces across the
ecosystem. They should remain media-agnostic whenever practical.

| Term | Definition | Usage guidance |
| --- | --- | --- |
| Platform | The shared product ecosystem that includes StoryQwest, SongQwest, creator tooling, audience surfaces, marketplace/community surfaces, and shared account experiences. | Use for cross-product product strategy and shared capabilities. |
| Runtime | The production implementation that renders Qwests, manages state, validates content, publishes bundles, integrates services, and meets accessibility, testing, performance, and release standards. | Use for production systems, not lab prototypes or product navigation. |
| Experience | A user-facing surface or workflow, such as Browse, Library, Reader, Listener, Cover Art Studio, or Media Library. | Prefer this over rigid account-type language when describing how a person is using the platform. |
| Home | The primary signed-in landing surface for returning to recent activity and relevant next actions. | Use for account-level orientation, not in-Qwest navigation. |
| Browse | The platform surface for finding Qwests, creators, collections, and featured content. | Prefer Browse over Discover for navigation because Discovery is reserved Qwest terminology. |
| Library | A user's saved, owned, claimed, started, bookmarked, or otherwise available Qwests and collections. | Use for durable access and resume surfaces. |
| Collections | Curated or user-organized groups of Qwests, creators, themes, or media-agnostic platform items. | Use for platform grouping; do not use Discovery Collections for navigation. |
| Profile | A user or creator identity surface containing public, private, account, and visibility information depending on context. | Qualify as Public Profile, Creator Profile, or Account Profile when ambiguity matters. |
| Notifications | Platform-level messages, alerts, updates, publishing events, account events, and engagement prompts. | Keep separate from in-Qwest reveal or discovery feedback. |
| Settings | Account, accessibility, privacy, notification, playback, and experience preferences. | Keep platform settings distinct from runtime-specific reader/listener controls when needed. |
| Search | A platform capability for finding Qwests, creators, collections, library items, media, and authoring assets. | Qualify as Shared Search, Library Search, Media Search, or Runtime Search when scope matters. |

## StoryQwest Vocabulary

StoryQwest terms describe story-centered creation and audience experiences.

| Term | Definition | Usage guidance |
| --- | --- | --- |
| Reader | A person actively experiencing a StoryQwest, or the runtime surface for reading one when context is clear. | Use Audience when referring to the broader cross-media persona. |
| Author | A creator writing, structuring, testing, and publishing StoryQwest content. | Use Creator for the broader platform persona. |
| Manuscript | The authoring source for StoryQwest content before it becomes a published Qwest bundle. | Use for draft/story source material, not runtime output. |
| Chapter | A story-scale organizational unit inside a StoryQwest manuscript or reader experience. | Use when story structure is chapter-based; do not require every Qwest to use chapters. |

## SongQwest Vocabulary

SongQwest terms describe song-centered creation and audience experiences.

| Term | Definition | Usage guidance |
| --- | --- | --- |
| Listener | A person actively experiencing a SongQwest, or the runtime surface for listening when context is clear. | Use Audience when referring to the broader cross-media persona. |
| Songwriter | A creator composing or structuring song-centered content. | Use when the creative role is specifically songwriting. |
| Artist | A performer, recording artist, or public creative identity attached to SongQwest content. | Do not use as a generic replacement for Creator when the work may be literary. |
| Performance | The timed SongQwest experience of audio, lyrics, media, ambience, branches, discoveries, and interaction lanes. | Use for listener/runtime experience, not platform browsing. |
| Playlist | An ordered collection of songs, performances, or SongQwest experiences. | Use only when ordering and playback sequence matter. |

## Shared Qwest Vocabulary

Shared Qwest terms describe mechanics that can exist inside every Qwest
regardless of media type.

| Term | Definition | Usage guidance |
| --- | --- | --- |
| Qwest | A published interactive creative work that can include story, song, media, state, branches, decisions, reveals, discoveries, journal entries, map context, cast context, and progression. | Reserved as the core content unit across the platform. |
| Journal | The in-Qwest record of meaningful discovered, unlocked, remembered, or creator-authored context. | Reserved for Qwest mechanics; do not use for platform blog or activity feed without qualification. |
| Map | A Qwest-level representation of places, paths, journey structure, or progression geography. | Qualify as Journey Map, World Map, or Song Journey Map when useful. |
| Cast | The Qwest-level set of characters, performers, identities, or entities relevant to the experience. | Reserved for in-Qwest participants, not account teams or app staff. |
| Reveal | An interaction that exposes hidden, staged, or additional content inside a Qwest. | Reserved for Qwest mechanics. |
| Discovery | An interactive Qwest content item, clue, secret, lore item, media unlock, collectible, or meaningful optional find. | Reserved for Qwest mechanics; do not use Discover/Discovery as top-level platform navigation. |
| Timeline | A structured sequence of Qwest state, memory, performance, progression, or timed events. | Qualify as Reader Memory Timeline, Performance Timeline, or Runtime Timeline when scope matters. |
| Branch | A Qwest path split, conditional route, alternate path, or graph edge that changes progression. | Use for content graph structure, not source-control branches unless explicitly in engineering context. |
| Decision | A user choice or authored condition that affects Qwest progression, state, or experience. | Use Choice for the UI affordance when needed; use Decision for the meaningful outcome. |

## Reserved Terms

The following terms are reserved because they represent specific Qwest
mechanics:

- **Discovery / Discoveries**
- **Reveal**
- **Journal**
- **Cast**
- **Qwest**

"Discovery" and "Discoveries" are reserved Qwest terminology referring to
interactive content inside a Qwest and should not be reused for platform
navigation.

Use these platform navigation labels instead:

- Browse
- Home
- Library
- Featured
- Collections
- Search
- Recommendations

When a historical document uses Discover or Discovery for marketplace or library
navigation, treat that wording as legacy product language. New planning should
use Browse, Featured, Collections, Search, or Recommendations unless it is
describing an in-Qwest discovery mechanic.

## Experience Personas

These personas describe the primary ways people interact with the platform.
They are experience personas, not mutually exclusive account types. A single
account may move fluidly between Explorer, Audience, and Creator experiences
during the same session.

### Explorer

Someone discovering content throughout the platform.

Typical experiences:

- Browse
- Search
- Recommendations
- Collections
- Featured Content

Primary goal: find stories, songs, creators, and collections to experience.

### Audience

Someone actively experiencing content.

Typical experiences:

- Reader
- Listener
- Library
- Bookmarks
- Reading or Listening Progress
- Journal
- Maps
- Cast
- Reveals
- Discoveries

Primary goal: immerse themselves in StoryQwest and SongQwest experiences.

### Creator

Someone producing and publishing content.

Typical experiences:

- Story Authoring
- Song Authoring
- Cover Art Studio
- Publishing
- Analytics
- Asset Library
- Media Library

Primary goal: create, refine, publish, and manage creative works.

## Naming Principles

- Prefer immersive language over generic software terminology when it clarifies
  the experience.
- Avoid duplicate meanings. One term should not mean both a platform surface and
  an in-Qwest mechanic.
- Reserve domain terminology for Qwest mechanics.
- Keep platform vocabulary distinct from runtime vocabulary.
- Shared experiences should remain media-agnostic whenever practical.
- Qualify terms when context could be ambiguous, such as Public Profile,
  Creator Profile, Runtime Timeline, or Media Library.
- Do not rename runtime components casually. Existing component names may remain
  until a future promotion or migration plan explicitly changes them.
