# Canonical Content Model

> **Documentation class: Canonical.** This document defines the durable published Qwest bundle boundary and Qwest-level metadata contract. It was separated from the [Phase 4 implementation specification](../../story-song-qwest/docs/songqwest-storyqwest-phase-4-unified-content-model-runtime-platorm.md) so later phase status cannot silently redefine the contract.

[Content Model Overview](overview.md) · [Presentation](presentation.md) · [Manuscript](manuscript.md) · [Sections](sections.md) · [Runtime Graph](runtime-graph.md) · [Media](media.md) · [Interactions](interactions.md) · [World](world.md) · [Maps](maps.md) · [Journal](journal.md) · [Progress](progress.md) · [Validation](validation.md)

## Core Rule

Authoring, preview, publishing, validation, and runtime operate against the same
canonical structure. The published bundle is the runtime contract; individual
surfaces may project different views of it but must not invent competing models.

The model separates three concerns:

- **Narrative sequence** advances through the runtime graph.
- **Interactive content** lets the audience inspect, discover, and act without
  implicitly changing narrative flow.
- **Persistent world and progress** retain world knowledge and reader state
  independently of renderer details.

## Three-Axis Section Architecture

Structural sections use three orthogonal concepts:

- **`NodeType` — runtime graph behavior.** `nodeType: "section"` identifies a
  readable section node that participates in addressing, flow, validation,
  progress, Journey Maps, and replay.
- **`SectionType` — semantic content.** Optional `sectionType` metadata describes
  whether a section contains a scene, song, dialogue, journal, or letter.
- **`SectionRole` — structural placement.** Optional `sectionRole` metadata
  identifies a prologue, epilogue, or credits section within the overall work.

```ts
{
  nodeType: "section",
  sectionType: "song",
  sectionRole: "prologue"
}
```

`SectionType` and `SectionRole` do not create runtime node types, graph edges,
or progression behavior. When `sectionType` is omitted, the node is a general
narrative section. When `sectionRole` is omitted, the section belongs to the
main body. Runtime traversal continues to use stable node IDs, `NodeType`, and
explicit compiled flow.

## Document Map

- [Manuscript Model](manuscript.md) separates authored order from runtime flow
  and defines nodes, transitions, targets, and choices.
- [Qwest Media Model](media.md) defines media identity, metadata, authored
  relationships, publishing, delivery boundaries, playback intent, and
  accessibility.
- [Runtime Graph and Services](runtime-graph.md) defines runtime organization,
  resolution, registered extensions, reader atmosphere, and safe containers.
- [Interactions, Discoveries, and Scoring](interactions.md) defines interactive
  text, reveals, discoveries, achievements, and scoring.
- [World and Environmental Model](world.md) defines reusable world entities and
  authored environmental intent.
- [Maps and Spatial Narrative](maps.md) defines spatial records, authored maps,
  Journey Maps, navigation, and exploration.
- [Journal, Lore, and World Knowledge](journal.md) defines authored knowledge
  objects and their discovery lifecycle.
- [Progress Model](progress.md) separates per-Qwest progress from cross-Qwest
  user state and renderer session state.
- [Structural Sections](sections.md) defines the structural authoring model
  shared by StoryQwest and SongQwest.
- [Content Model Validation](validation.md) defines cross-surface integrity and
  acceptance requirements.

## Canonical Qwest Schema

The published bundle becomes the canonical runtime contract.

```ts
type QwestBundle = {
  schemaVersion: number;
  qwest: QwestMetadata;
  nodes: QwestNode[];
  media: MediaAsset[];
  interactions: InlineInteraction[];
  contentTargets: ContentTarget[];
  regions?: Region[];
  locations?: Location[];
  worldEntities?: WorldEntity[];
  maps?: QwestMap[];
  characters?: Character[];
  journalEntries?: JournalEntry[];
  discoveries: Discovery[];
  achievements: Achievement[];
  endings: Ending[];
  ambientEffects?: AmbientEffectRef[];
  ambientRules?: AmbientRule[];
  ambientTags?: AmbientTag[];
  ambientZones?: AmbientZone[];
  ambientAssist?: AmbientAssistDefaults;
};
```

Product copy should use Cast and Cast member. `worldEntities` is the forward-looking canonical contract for Cast and other world identity records. The published runtime contract may keep `Character`, `characters`, and `type: "character"` names for compatibility while older consumers migrate.

Product copy should use Journal/Lore for reader-facing and Creator-facing knowledge surfaces. Journal is the reader-facing and Creator-facing product surface. Lore, notes, books, letters, and documents are entry types within the Journal system. `journalEntries` is the canonical runtime contract for collected journals, lore pages, notes, books, letters, documents, and future Qwest Journal & World Knowledge records.

All platform surfaces should consume this structure.

The `media` registry and all media-backed relationships are governed by the
canonical [Qwest Media Model](media.md).

### Qwest Artwork

Qwest metadata may reference the `Artwork` contract defined by the canonical
[Qwest Media Model](media.md#artwork). Artwork remains media-backed presentation
metadata and never affects narrative behavior.

### Qwest Theme

Qwest metadata may declare an optional reader story theme:

```ts
type QwestMetadata = {
  id: string;
  title: string;
  artwork?: Artwork;
  readerTheme?: ReaderTheme;
  theme?: ReaderTheme;
};
```

`readerTheme` is the preferred property name because the theme applies only to the reader story surface. `theme` is allowed as a compatibility alias if existing content, importers, or tools already use that name. Published bundles should normalize to `readerTheme` where possible.

The authored presentation axes and their runtime ownership are defined in
[Qwest Presentation Metadata](presentation.md). In particular, a Foundation
environment is distinct from a Reader preset, reading surface, and accent.
