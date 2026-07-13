# Product Language

> **Documentation class: Canonical.** This document governs long-lived product
> vocabulary. Current phase status belongs in roadmaps and readiness documents;
> dated terminology decisions belong in historical evidence. See
> [Documentation Taxonomy](documentation-taxonomy.md).

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

## Canonical SongQwest Creator Workflow

SongQwest is not a digital audio workstation. Artists record, edit, mix, master,
and otherwise produce music in professional tools outside SongQwest. Creator
Studio begins after the music is complete and references imported audio assets;
it does not modify their waveforms, stems, or mixes.

The canonical workflow is:

1. Produce music outside SongQwest.
2. Import finalized MP3, WAV, FLAC, AAC, or other common release-ready audio.
3. Organize imported songs into StoryQwest-style scenes or sections.
4. Attach lyrics to each scene.
5. Enrich lyrics and scenes with applicable media moments, reveals,
   discoveries, journal entries, lore, cast references, locations, maps, inline
   interactions, collectibles, achievements, companions, and environmental
   storytelling.
6. Add optional branches containing alternate finished songs, completed spoken
   narration or dialogue, narrated choices, or alternate story paths.
7. Continue building scenes and branches until the experience is complete.
8. Polish with existing runtime systems such as ambience, environments, maps,
   companions, collectibles, progression, discoveries, runtime behaviors, and
   presentation.
9. Preview the production runtime, validate publish readiness, and publish the
   completed interactive SongQwest.

```text
Professional Music Tools
          |
          v
Finished Song
(MP3 | WAV | FLAC | AAC)
          |
          v
Import into SongQwest
          |
          v
Scenes / Sections
          |
          v
Lyrics
          |
          v
Interactive Enrichment
          |
          v
Branches
          |
          v
Preview
          |
          v
Publish
```

Use **audio assets**, **imported songs**, **finalized recordings**,
**release-ready music**, **scenes**, **sections**, **lyrics**, **interactive
content**, **enrichment**, **preview**, and **publish** for this workflow.
Recording, waveform or audio-timeline editing, stem editing, alternate mixes,
adaptive music production, MIDI editing, mixing, mastering, and live-performance
production are outside the canonical workflow, including the Public Beta
baseline. Any future exploration of those capabilities is optional.

## Related Documents

- `docs/architecture-map.md` governs workspace architecture, repository
  ownership, feature routing, and promotion ownership.
- `docs/system-map.md` summarizes the Platform, Runtime, and Qwest terminology
  layers in the workspace dependency flow.
- `qwest-experience-platform/README.md` describes the active Experience Lab,
  bundle tooling, shared platform models, Bundle Explorer, Bundle Inspector, and
  foundation work.
- `qwest-design-system/docs/experience-lab-promotion-audit.md` records
  historical Experience Lab planning status and foundational shared experience
  candidates from the legacy prototype repository.
- `story-song-qwest/docs/songqwest-storyqwest-phase-5-runtime-promotion-roadmap.md`
  governs how validated Experience Lab work becomes production runtime.

## Platform Vocabulary

Platform terms describe navigation and durable product surfaces across the
ecosystem. They should remain media-agnostic whenever practical.

---

### **Platform & Core Systems**

- **Platform**
- **Definition:** The shared product ecosystem and application boundary that includes StoryQwest, SongQwest, creator tooling, audience surfaces, hosting, registration, lifecycle, shared services, marketplace/community surfaces, and shared account experiences.
- **Usage Guidance:** Use for cross-product product strategy and shared capabilities.

- **Runtime**
- **Definition:** The production implementation that renders Qwests and owns runtime behavior, runtime state, runtime progress, runtime media, runtime accessibility, validation, publishing, teardown, testing, performance, and runtime release evidence. It does not name the surrounding shared Platform services.
- **Usage Guidance:** Use for production systems, not lab prototypes or product navigation.

- **Experience**
- **Definition:** A cohesive way a person interacts with the platform. An experience may include multiple screens, workflows, runtime components, and Qwest mechanics working together toward a single purpose.
- **Usage Guidance:** Prefer this over rigid account-type or page-level language. Reader, Listener, Story Authoring, Library, and Cover Art Studio are experiences, not just pages.

---

### **Platform Surfaces & Navigation**

- **Home**
- **Definition:** The primary signed-in landing surface for returning to recent activity and relevant next actions.
- **Usage Guidance:** Use for account-level orientation, not in-Qwest navigation.

- **Browse**
- **Definition:** The platform surface for finding Qwests, creators, collections, and featured content.
- **Usage Guidance:** Prefer Browse over Discover for navigation because Discovery is reserved Qwest terminology.

- **Library**
- **Definition:** The user's persistent content hub across the platform, including saved, owned, claimed, started, bookmarked, drafted, published, and reusable creative materials.
- **Usage Guidance:** Use for durable access, resume surfaces, creator work, and reusable assets. StoryQwest and SongQwest may expose specialized Library views over the same underlying platform concept.

- **Collections**
- **Definition:** Curated or user-organized groups of Qwests, creators, themes, or media-agnostic platform items.
- **Usage Guidance:** Use for platform grouping; do not use Discovery Collections for navigation.

---

### **Identity & Utilities**

- **Profile**
- **Definition:** A user or creator identity surface containing public, private, account, and visibility information depending on context.
- **Usage Guidance:** Qualify as Public Profile, Creator Profile, or Account Profile when ambiguity matters.

- **Search**
- **Definition:** A platform capability for finding Qwests, creators, collections, library items, media, and authoring assets.
- **Usage Guidance:** Qualify as Shared Search, Library Search, Media Search, or Runtime Search when scope matters.

- **Notifications**
- **Definition:** Platform-level messages, alerts, updates, publishing events, account events, and engagement prompts.
- **Usage Guidance:** Keep separate from in-Qwest reveal or discovery feedback.

- **Settings**
- **Definition:** Account, accessibility, privacy, notification, playback, and experience preferences.
- **Usage Guidance:** Keep platform settings distinct from runtime-specific reader/listener controls when needed.

### Experience Scope

Experiences are broader than individual pages or routes. A single experience may
span navigation, panels, modals, runtime components, persisted state, creator
workflows, Qwest mechanics, and supporting platform services.

Examples:

- **Reader** is an Audience experience that may include reading surfaces,
  choices, reveals, discoveries, journal updates, maps, cast context, progress,
  settings, and runtime state.
- **Listener** is an Audience experience that may include audio, lyrics,
  transport controls, performance timelines, reveals, discoveries, ambience,
  progress, and accessibility fallbacks.
- **Story Authoring** is a Creator experience that may include manuscript
  drafting, structure, branching, validation, preview, publishing, media, and
  analytics.
- **Library** is a persistent platform experience that may include stories,
  songs, drafts, published works, collections, artwork, cover art, audio,
  manuscripts, media assets, bookmarks, Continue Reading, and Continue
  Listening.
- **Cover Art Studio** is a Creator experience that may include visual asset
  generation, editing, review, reuse, and publishing preparation.

## Platform Experience Architecture

Platform Experience Architecture describes how experience personas relate to
shared platform experiences. These relationships are conceptual planning
relationships, not permission roles, security roles, entitlement rules, or
final navigation labels.

A user may move fluidly between Explorer, Audience, and Creator experiences
during a single session.

| Platform Experience | Explorer | Audience | Creator |
| ------------------- | -------- | -------- | ------- |
| Home                | x        | x        | x       |
| Browse              | x        |          |         |
| Library             |          | x        | x       |
| Reader              |          | x        |         |
| Listener            |          | x        |         |
| Story Authoring     |          |          | x       |
| Song Authoring      |          |          | x       |
| Cover Art Studio    |          |          | x       |
| Profile             | x        | x        | x       |
| Collections         | x        | x        | x       |
| Search              | x        | x        | x       |
| Notifications       |          | x        | x       |
| Settings            | x        | x        | x       |

Planning notes:

- **Song Authoring** is retained as an existing planning label in this
  document. It means authoring an interactive SongQwest experience around
  imported, finalized music—not authoring or producing the music itself.
- A future terminology review may consider **Song Experience Authoring**,
  **SongQwest Authoring**, or **Interactive Song Authoring**. That naming review
  must not change architecture or roadmap scope.

- Explorer relationships emphasize finding, evaluating, and following creative
  work.
- Audience relationships emphasize active reading, listening, saving,
  resuming, and revisiting.
- Creator relationships emphasize drafting, managing, publishing, measuring,
  and maintaining creative work.
- The matrix should not be used to infer authorization. Runtime permissions,
  entitlements, privacy, and visibility require separate production contracts.

### Library As Persistent Content Hub

Library is the user's persistent content hub across StoryQwest, SongQwest, and
future Qwest formats. It is broader than a shelf of saved Qwests.

Library may include:

- Stories
- Songs
- Drafts
- Published Works
- Collections
- Artwork
- Cover Art
- Audio
- Manuscripts
- Media Assets
- Bookmarks
- Continue Reading
- Continue Listening

StoryQwest and SongQwest can surface specialized views of Library while still
sharing the same platform concept. For example, a StoryQwest Library view may
prioritize manuscripts, reading progress, chapters, and story collections,
while a SongQwest Library view may prioritize songs, listening progress, audio,
performances, and playlists.

### Studio Planning

Cover Art Studio is currently documented as a Creator experience. In future
platform architecture, it may become one area within a broader **Studio**
experience.

Studio may eventually include:

- Cover Art Studio
- Asset Library
- Media Library
- Publishing
- Analytics
- Creator Tools

This is future planning only. It does not rename existing Experience Lab
terminology, runtime components, routes, navigation, or promotion plans.

## StoryQwest Vocabulary

StoryQwest terms describe story-centered creation and audience experiences.

| Term       | Definition                                                                                                 | Usage guidance                                                                         |
| ---------- | ---------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------- |
| Reader     | A person actively experiencing a StoryQwest, or the runtime surface for reading one when context is clear. | Use Audience when referring to the broader cross-media persona.                        |
| Author     | A creator writing, structuring, testing, and publishing StoryQwest content.                                | Use Creator for the broader platform persona.                                          |
| Manuscript | The authoring source for StoryQwest content before it becomes a published Qwest bundle.                    | Use for draft/story source material, not runtime output.                               |
| Chapter    | A story-scale organizational unit inside a StoryQwest manuscript or reader experience.                     | Use when story structure is chapter-based; do not require every Qwest to use chapters. |

## SongQwest Vocabulary

SongQwest terms describe song-centered creation and audience experiences.

| Term        | Definition                                                                                                      | Usage guidance                                                                 |
| ----------- | --------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------ |
| Listener    | A person actively experiencing a SongQwest, or the runtime surface for listening when context is clear.         | Use Audience when referring to the broader cross-media persona.                |
| Songwriter  | A creator composing or structuring song-centered content.                                                       | Use when the creative role is specifically songwriting.                        |
| Artist      | A performer, recording artist, or public creative identity attached to SongQwest content.                       | Do not use as a generic replacement for Creator when the work may be literary. |
| Performance | The timed SongQwest experience of audio, lyrics, media, ambience, branches, discoveries, and interaction lanes. | Use for listener/runtime experience, not platform browsing.                    |
| Playlist    | An ordered collection of songs, performances, or SongQwest experiences.                                         | Use only when ordering and playback sequence matter.                           |

## Shared Qwest Vocabulary

Shared Qwest terms describe mechanics that can exist inside every Qwest
regardless of media type.

| Term      | Definition                                                                                                                                                                                | Usage guidance                                                                                         |
| --------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------ |
| Qwest     | A published interactive creative work that can include story, song, media, state, branches, decisions, reveals, discoveries, journal entries, map context, cast context, and progression. | Reserved as the core content unit across the platform.                                                 |
| Journal   | The in-Qwest record of meaningful discovered, unlocked, remembered, or creator-authored context.                                                                                          | Reserved for Qwest mechanics; do not use for platform blog or activity feed without qualification.     |
| Map       | A Qwest-level representation of places, paths, journey structure, or progression geography.                                                                                               | Qualify as Journey Map, World Map, or Song Journey Map when useful.                                    |
| Cast      | The Qwest-level set of characters, performers, identities, or entities relevant to the experience.                                                                                        | Reserved for in-Qwest participants, not account teams or app staff.                                    |
| Reveal    | An interaction that exposes hidden, staged, or additional content inside a Qwest.                                                                                                         | Reserved for Qwest mechanics.                                                                          |
| Discovery | An interactive Qwest content item, clue, secret, lore item, media unlock, collectible, or meaningful optional find.                                                                       | Reserved for Qwest mechanics; do not use Discover/Discovery as top-level platform navigation.          |
| Timeline  | A structured sequence of Qwest state, memory, performance, progression, or timed events.                                                                                                  | Qualify as Reader Memory Timeline, Performance Timeline, or Runtime Timeline when scope matters.       |
| Branch    | A Qwest path split, conditional route, alternate path, or graph edge that changes progression.                                                                                            | Use for content graph structure, not source-control branches unless explicitly in engineering context. |
| Decision  | A user choice or authored condition that affects Qwest progression, state, or experience.                                                                                                 | Use Choice for the UI affordance when needed; use Decision for the meaningful outcome.                 |

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

## Emerging Platform Concepts

Emerging platform concepts are exploratory language and branding ideas. They
are not approved runtime terms, not current navigation labels, and not Qwest
mechanics unless later promoted through Experience Lab validation and
architecture review.

### Paths

**Paths** is a possible future branded platform concept for discovering and
following creative journeys. The intent is to explore language that feels more
native to StoryQwest and SongQwest than generic marketplace terminology.

Possible future phrasing:

| Current generic phrasing | Exploratory Path phrasing |
| ------------------------ | ------------------------- |
| Browse Stories           | Find your next Path       |
| Recommended for you      | Recommended Paths         |
| Featured Collections     | Curated Paths             |

Paths could eventually encompass recommendations, curated journeys, creator
spotlights, playlists, themed collections, reading sequences, listening
sequences, and cross-media experiences.

Guidance:

- Paths are not Qwest mechanics.
- Paths are not navigation labels today.
- Paths do not replace Browse, Search, Collections, Library, Reader, Listener,
  or any runtime component name.
- Paths remain exploratory until validated through the Experience Lab and
  accepted by future platform architecture planning.

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
