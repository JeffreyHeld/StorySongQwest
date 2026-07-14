# Journal, Lore, and World Knowledge

> **Documentation class: Canonical.** This document defines the durable author-created knowledge objects and their reader discovery lifecycle. It was separated from the [Phase 4 implementation specification](../../story-song-qwest/docs/songqwest-storyqwest-phase-4-unified-content-model-runtime-platorm.md) so later phase status cannot silently redefine the contract.

[Content Model Overview](overview.md) · [Manuscript](manuscript.md) · [Sections](sections.md) · [Runtime Graph](runtime-graph.md) · [Media](media.md) · [Interactions](interactions.md) · [World](world.md) · [Maps](maps.md) · [Journal](journal.md) · [Progress](progress.md) · [Validation](validation.md)

## Journal, Lore & World Knowledge

Phase 4U adds Journal/Lore as the "what" layer of the Phase 4 world foundation. Phase 4S defines Cast / World Entities as "who." Phase 4T defines Maps / Navigation / World Exploration as "where." Phase 4U defines journals, lore, notes, books, letters, and documents as the supplemental knowledge layer that authors can connect to story text, cast, locations, maps, media, discoveries, and other knowledge records.

Journal/Lore content is supplemental by default. It does not become runtime graph flow unless a specific journal or lore moment is explicitly represented by a `QwestNode`. It must not silently alter flow, choices, discoveries, achievements, score, or progress unless explicitly connected through existing discovery, condition, node, or choice systems.

### Objective

Define journals, lore, notes, books, letters, and documents as first-class supplemental knowledge objects that support worldbuilding, clues, history, character context, location context, and collectible reading experiences.

### Knowledge Object Model

`JournalEntry` is the canonical first object for this phase. The `entryType` field distinguishes journal, lore, note, book, letter, and document behavior without requiring separate runtime object families.

```ts
type JournalEntryType =
  | "journal"
  | "lore"
  | "note"
  | "book"
  | "letter"
  | "document";

type JournalEntry = {
  id: string;
  title: string;
  entryType: JournalEntryType;
  body: TextRun[];
  excerpt?: string;
  imageId?: string;
  mediaIds?: string[];
  tags?: string[];
  relatedTargetRefs?: ContentTargetRef[];
  visibility?: "visible" | "hidden" | "discovered";
  discoveryId?: string;
  sourceNodeId?: string;
};
```

Entry semantics:

- `journal` is a reader-facing collected entry.
- `lore` is world/background knowledge.
- `note` is short-form discovered text.
- `book` is long-form in-world text.
- `letter` is stylized correspondence.
- `document` is an artifact, record, report, clue, contract, recipe, map note, or other readable object.
- `visibility` controls whether the entry is initially available.
- `discoveryId` optionally links the entry to an existing Discovery so opening or finding the entry can mark a discovery.
- `sourceNodeId` optionally records where the entry was introduced or discovered.

`QwestBundle.journalEntries?: JournalEntry[]` carries these objects in the published runtime contract. Journal entries may reference cast members, locations, maps, media, discoveries, and other journal entries through `relatedTargetRefs` using the existing content target model.

### Interactive Text Integration

Highlighted reader text may target journal entries through the existing Interactive Elements model:

```ts
{
  type: "journal",
  target: {
    id: "journal-lantern-prophecy",
    type: "journal"
  }
}
```

Default activation should be `tap`.

Reader presentation should use the shared interaction viewer framework. Journal/Lore entries may open in a dialog, drawer, side panel, fullscreen surface, or a dedicated journal viewer depending on runtime surface and presentation context.

Interactive text must continue to target `ContentTargetType: "journal"` for journal and lore objects. Entry subtypes such as `lore`, `note`, `book`, `letter`, and `document` belong to `JournalEntry.entryType`; they do not create separate `ContentTargetType` values in Phase 4U.

### Reader Journal Viewer

A reader-facing journal viewer is an optional runtime surface. It can show:

- Discovered entries.
- Unread entries.
- Completed/read entries.
- Hidden locked entries if the author allows hints.
- Tags/categories.
- Related cast members.
- Related locations.
- Related maps.
- Related media.
- Related discoveries.
- Related journal entries.

The viewer must not be required for ordinary story reading. A Qwest can use inline journal interactions without exposing a global journal viewer.

### Discovery Integration

Journal/Lore uses existing discovery, condition, reader-state, and interaction-state systems. It must not create a second discovery system.

Automatic discovery integration:

- Opening a hidden or discovered journal entry may mark it as read.
- Finding a journal entry may optionally complete a linked Discovery.
- A journal entry may be unlocked by a discovery, achievement, flag, visited node, visited location, or other existing condition.
- Journal/Lore entries may support replay and completion tracking through existing `ReaderState` and `InteractionState`.

Read/unread/completed behavior should use the existing `InteractionState` model for opened, read, and completed interactions. Discovery completion should use existing `Discovery` records and reader discovery state.

### Validation

Validation rules:

- Journal entry IDs must be stable and unique within a Qwest.
- Journal entry `entryType` must be valid.
- Journal entry body must contain valid `TextRun[]`.
- Journal `imageId` and `mediaIds` must resolve to valid media assets.
- `relatedTargetRefs` must resolve to valid content targets.
- `discoveryId` must resolve to a valid Discovery when present.
- `sourceNodeId` must resolve to a valid QwestNode when present.
- Interactive text targets of type `journal` must resolve to valid journal entries.
- Hidden journal entries must have a valid unlock, discovery path, or intentional hidden state.
- Deleting a journal entry referenced by interactive text must create a validation error until the reference is removed or reassigned.
- Journal entries must not alter runtime graph flow unless explicitly connected through existing node, discovery, condition, or choice mechanisms.

### Acceptance Criteria & Tests

#### Acceptance Criteria

- A Qwest can define first-class journal, lore, note, book, letter, and document entries.
- Interactive text can open a journal/lore entry through the shared interaction system.
- Reader state can track opened/read/completed journal interactions.
- Hidden journal entries can be discovered or unlocked through existing condition/discovery systems.
- A journal entry can optionally complete a linked Discovery.
- Journal entries can reference cast members, locations, maps, media, discoveries, and other journal entries through content targets.
- The reader journal viewer can show discovered, unread, read, and related entries.
- Journal/Lore content remains supplemental by default and does not alter runtime flow unless explicitly wired through existing systems.

#### Required Tests

- Unit Test: Resolve a valid journal content target.
- Unit Test: Resolve related journal target references.
- Validation Test: Reject duplicate journal entry IDs.
- Validation Test: Reject journal entries with invalid `entryType`.
- Validation Test: Reject journal entries with missing media references.
- Validation Test: Reject journal entries with missing discovery references.
- Validation Test: Reject interactive text targeting a deleted journal entry.
- Interaction Test: Opening journal-linked text records an `InteractionState`.
- Runtime/export Test: Published bundles include valid `journalEntries`.
- Reader Test: Journal viewer displays discovered, unread, and read entries.
- Discovery Test: Opening or finding a journal entry can complete a linked Discovery when configured.
