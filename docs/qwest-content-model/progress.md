# Progress Model

> **Documentation class: Canonical.** This document defines the durable per-Qwest progress, cross-Qwest user state, persistence, reset, replay, and synthetic test-data boundaries. It was separated from the [Phase 4 implementation specification](../../story-song-qwest/docs/songqwest-storyqwest-phase-4-unified-content-model-runtime-platorm.md) so later phase status cannot silently redefine the contract.

[Content Model Overview](overview.md) · [Manuscript](manuscript.md) · [Sections](sections.md) · [Runtime Graph](runtime-graph.md) · [Media](media.md) · [Interactions](interactions.md) · [World](world.md) · [Maps](maps.md) · [Journal](journal.md) · [Progress](progress.md) · [Validation](validation.md)

## Qwest Reader State

Reader state is the canonical per-Qwest progress model.

The persisted runtime document should be treated conceptually as `QwestProgress`: one player, one Qwest, one saved progress record. It represents durable Qwest progress, not merely transient in-memory runtime state.

The reader runtime may keep additional ephemeral session state while rendering, animating, preloading, or resolving services. Those implementation details are outside the canonical content contract unless they need to be restored across sessions.

```ts
type ReaderState = QwestProgress;

type QwestProgress = {
  id: string;
  playerId?: string;
  qwestId: string;
  schemaVersion: number;
  publication: ProgressPublicationIdentity;
  versionContinuity?: ProgressVersionContinuity;
  /** Temporary compatibility field for older persisted documents. */
  qwestVersion?: string;

  navigation: ProgressNavigation;
  storyState: StoryState;
  scoring: ProgressScoring;
  statistics: ProgressStatistics;
  readerContent: ProgressReaderContent;
  runtimeState?: ProgressRuntimeState;
  runtimeData?: Record<string, unknown>;

  firstOpenedAt: string;
  lastOpenedAt: string;
  createdAt: string;
  savedAt: string;
  updatedAt: string;
};
```

`schemaVersion` identifies the persisted progress-document shape. It is not a
published-content version. `publication.versionId` identifies the immutable
published bundle against which the progress was last validated and saved.

```ts
type ProgressPublicationIdentity = {
  versionId: string;
  firstOpenedVersionId?: string;
  bundleHash?: string;
};

type ProgressVersionContinuity = {
  evaluatedVersionId?: string;
  status:
    | "current"
    | "compatible"
    | "migrated"
    | "confirmationRequired"
    | "restartRequired"
    | "legacyVersion"
    | "unavailable";
  migration?: ProgressMigrationRecord;
  retainedVersionId?: string;
};

type ProgressMigrationRecord = {
  fromVersionId: string;
  toVersionId: string;
  migratedAt: string;
  outcome:
    | "exact"
    | "textRunFallback"
    | "sectionFallback"
    | "nodeFallback"
    | "firstReadableFallback"
    | "restartRequired";
  reason?:
    | "publicationUpdated"
    | "nodeRemoved"
    | "textRunRemoved"
    | "mediaChanged"
    | "choiceChanged"
    | "runtimeCompatibilityChanged";
  previousLocation?: ProgressLocationSnapshot;
  resolvedLocation?: ProgressLocationSnapshot;
};

type ProgressLocationSnapshot = {
  nodeId?: string;
  textRunId?: string;
  playbackPositionMs?: number;
};
```

The deprecated `qwestVersion` field is read only during bounded legacy
migration. New writes use `publication.versionId`; a missing publication
identity enters explicit legacy resolution rather than being assumed current.
Legacy migration is idempotent and must not invent `firstOpenedVersionId`.

`QwestProgress` is logically divided into navigation, story state, scoring, statistics, and reader-created content. This keeps the persisted document extensible without turning the root object into a flat list of unrelated fields.

This also separates story progress from player progress concerns. Navigation and `StoryState` describe where the player is in the authored Qwest and what story content has been unlocked, found, or reached. Scoring and statistics describe measured player activity. `ProgressReaderContent` stores reader-created material such as bookmarks and notes. `ProgressRuntimeState` stores document lifecycle state that does not come from authored story flags.

The optional `runtimeData` field is reserved for future runtime-owned systems that need persistence without changing the root object again, such as inventory, companions, quests, collectibles, or currencies. Extension data must remain runtime-owned and validated; authored story logic should not depend on arbitrary extension payloads without a future explicit contract.

```ts
type ProgressNavigation = {
  currentNodeId: string;
  currentTextRunId?: string;
  currentReadingPosition?: ReadingPosition;
  scrollOffset?: ScrollOffset;
  resumeLocation?: ResumeLocation;

  visitedNodes: string[];
  visitedLocations?: string[];
  journeyPath?: JourneyPathEntry[];
  pathHistory?: PathHistoryEntry[];
  choicesMade?: ChoiceSelection[];
};

type StoryState = {
  flags: Record<string, unknown>;
  unlocks: string[];
  discoveries: string[];
  revealsUnlocked: string[];
  achievements: string[];
  endingsReached: string[];
  interactionState: Record<string, InteractionState>;
};

type ProgressScoring = {
  scoreSummary: ScoreSummary;
  scoreEvents: ScoreEvent[];
};

type ProgressReaderContent = {
  bookmarks?: ProgressBookmark[];
  notes?: ReaderNote[];
};

type ProgressRuntimeState = {
  completed?: boolean;
  abandoned?: boolean;
  imported?: boolean;
  replayMode?: boolean;
  synthetic?: boolean;
};
```

`StoryState.flags` belongs to authored story logic. `ProgressRuntimeState` belongs to the progress document itself and should be reserved for runtime lifecycle status such as completion, abandonment, import, replay, or synthetic-test mode.

```ts
type ReadingPosition = {
  nodeId: string;
  textRunId?: string;
  characterOffset?: number;
  wordOffset?: number;
  percentThroughNode?: number;
};

type ScrollOffset = {
  top: number;
  left?: number;
  containerId?: string;
};

type ResumeLocation = {
  nodeId: string;
  textRunId?: string;
  readingPosition?: ReadingPosition;
  scrollOffset?: ScrollOffset;
  savedAt: string;
};
```

```ts
type JourneyPathEntry = {
  nodeId: string;
  locationId?: string;
  position?: MapPosition;
  visitedAt?: string;
};

type PathHistoryEntry = {
  fromNodeId?: string;
  toNodeId: string;
  choiceId?: string;
  transitionId?: string;
  locationId?: string;
  enteredAt: string;
};

type ChoiceSelection = {
  choiceId: string;
  fromNodeId: string;
  toNodeId?: string;
  selectedAt: string;
  label?: string;
};
```

```ts
type ProgressBookmark = {
  id: string;
  target: AddressableTarget;
  label?: string;
  createdAt: string;
};

type ReaderNote = {
  id: string;
  target: AddressableTarget;
  body: string;
  createdAt: string;
  updatedAt?: string;
};

type ProgressStatistics = {
  textRunsCompleted?: number;
  textRunsReread?: number;
  wordsCompleted?: number;
  wordsReread?: number;
  nodesVisited?: number;
  choicesMade?: number;
  discoveriesFound?: number;
  revealsUnlocked?: number;
  endingsReached?: number;
  sessionsStarted?: number;
  totalReadingTimeMs?: number;
  completionPercent?: number;
};

type ScoreSummary = {
  totalScore: number;
  discoveryCount: number;
  achievementCount: number;
  revealCount: number;
  endingCount: number;
  completionPercent?: number;
  recalculatedAt?: string;
};
```

Reader notes are player-authored content. They are intentionally named `ReaderNote` to keep them distinct from author-created journal notes, lore notes, annotations, or future creator-authored supplemental material.

Reader progress becomes a first-class system.

Every structural Manuscript Section is a `QwestNode`, so progress identifies it
through `currentNodeId` and `nodeId`. `SectionType` and `SectionRole` are not
progress keys. Changing either field while preserving the node ID does not
invalidate navigation, resume, scoring, Journey paths, path history, or replay.
Song subdivisions such as Intro, Verse, and Chorus use Song-specific segment
metadata rather than a structural `sectionId` in Qwest progress.

### Progress Persistence

Runtime implementations own persistence mechanics.

This document defines the canonical progress contract that persistence must preserve.

Persistence behavior should support:

- autosave after meaningful progress events, including first open, last open, node entry, text-run completion, text-run reread, choice selection, discovery, reveal, achievement, ending, bookmark, and reader note creation
- resume from `resumeLocation` when present, with fallback to the latest valid `navigation.currentNodeId` and `navigation.currentReadingPosition`
- offline compatibility through a local durable progress document that can later synchronize with an authenticated profile
- synchronization boundaries that merge progress events and summaries without allowing stale device state to erase newer real progress
- document versioning through `schemaVersion`, separate from publication identity
- migration from older progress documents into the current `QwestProgress` shape
- future cloud sync without changing the Qwest content model
- reset behavior that clears Qwest-specific progress without deleting global reader preferences or unrelated UserState aggregates
- replay generation from `pathHistory`, `journeyPath`, choices, discoveries, reveals, endings, and score events

Autosave must preserve intent, not renderer detail. For example, scroll position may be saved as a resume hint, but the canonical progress record should still be valid if a future runtime resumes by node and text run instead.

When a saved position references content that no longer exists in the current published Qwest version, the runtime should migrate to the closest valid ancestor or next readable node and record that migration as implementation metadata.

### Published Version Continuity

Every successful publish creates a new immutable bundle and updates the Qwest's
current-published pointer only after the bundle is available. Publishing never
rewrites active audience progress and never mutates a prior published bundle.

Fresh starts resolve the current published version. Continue first compares the
saved `publication.versionId` with that current version, before mounting normal
runtime autosave. Matching progress continues normally. Different versions are
evaluated with both immutable bundles and then resolved as compatible,
confirmation-required, restart-required, legacy, or unavailable.

The Runtime Host owns version selection, comparison, persistence gating, and the
audience decision. StoryQwest owns node, text-run, choice, discovery, reveal,
ending, bookmark, and note resolution. SongQwest owns structural context,
playback checkpoints, playback position, lyrics/transcript fallbacks, and
Song-specific runtime data. The host does not interpret those detailed runtime
fields, and runtimes do not select the current publication independently.

Compatible migration updates `publication.versionId` only after validation
succeeds and records a diagnostic `ProgressMigrationRecord`. Material but
recoverable changes may ask the audience whether to continue from the closest
matching place or restart. Incompatible changes preserve the prior canonical
progress, offer safe restart or Library actions, and may retain the exact prior
immutable bundle for bounded legacy continuation. A restart stores a recoverable
snapshot before replacing the active progress. Migration writes use revision or
publication ordering checks so stale devices cannot move a completed migration
backward.

StoryQwest fallback order is saved text-run position, current text run, current
node start, authored ancestor/successor, first readable node, then restart
required. SongQwest fallback order is saved checkpoint, saved playback position,
Song/structural section start, structural node start, first playable section,
first readable section, then restart required. A changed media reference alone
does not reset the full Qwest when the structural section remains valid.

QwestProgress should remain the canonical public persistence contract: one player, one Qwest, one progress document. Implementations should not prematurely expose bookmarks, score events, choices, or other progress components as separate public source-of-truth documents. If a runtime eventually needs internal sharding for storage limits or synchronization performance, it may do so behind this contract while preserving the same logical QwestProgress shape.

Future save-slot behavior may be layered above QwestProgress without replacing it.

```ts
type SaveSlot = {
  id: string;
  label?: string;
  progressId: string;
};
```

Save slots are a reserved future concept for manual saves, named checkpoints, replay from a prior state, and alternate-ending exploration. A save slot points at a progress document or snapshot; it is not itself the source of truth for story state, scoring, bookmarks, or reader-created content.

## User State

UserState aggregates information across Qwests.

It must not duplicate detailed per-Qwest progress owned by `QwestProgress`.

```ts
type UserState = {
  profile: ReaderProfile;
  progress: UserProgress;
  preferences: ReaderPreferences;
};

type ReaderProfile = {
  id: string;
  type: "anonymous" | "authenticated";
  displayName?: string;
  avatarUrl?: string;
  createdAt?: string;
  updatedAt?: string;
};

type UserProgress = {
  qwestsStarted: string[];
  qwestsCompleted: string[];
  discoveries: UserDiscoverySummary[];
  achievements: UserAchievementSummary[];
  totalScore: number;
  totalDiscoveries?: number;
  totalAchievements?: number;
  favoriteQwests?: string[];
  recentlyPlayed?: RecentQwest[];
};

type UserDiscoverySummary = {
  qwestId: string;
  discoveryId: string;
  foundAt: string;
};

type UserAchievementSummary = {
  qwestId: string;
  achievementId: string;
  earnedAt: string;
};

type RecentQwest = {
  qwestId: string;
  lastVisitedAt: string;
  progressPercent?: number;
};

type ReaderPreferences = {
  theme?: "light" | "dark" | "system";
  fontSize?: "small" | "medium" | "large";
  highContrast?: boolean;
  audioEnabled?: boolean;
  reducedMotion?: boolean;
  disableAmbience?: boolean;
  ambientVisualsEnabled?: boolean;
  ambientAudioEnabled?: boolean;
  ambientLightingEnabled?: boolean;
  ambientHapticsEnabled?: boolean;
  ambientVolume?: number;
};
```

### Progress Boundaries

ReaderState / QwestProgress belongs to a specific Qwest.

UserState belongs to the reader.

ReaderState may be reset without affecting UserState.

UserState may aggregate:

- discoveries
- achievements
- score
- completion history
- journey history
- qwests started
- qwests completed
- recently played Qwests
- favorite Qwests

Qwest content must never directly modify UserState.

Runtime systems derive UserState from ReaderState events.

Qwest-specific reading progress remains owned by the Qwest Progress document.

## Experience Lab Progress Utilities

Experience Lab may inspect, validate, visualize, and generate synthetic progress documents for testing.

Experience Lab is never the source of truth for real player progress.

Synthetic progress generation may derive test progress from a published Qwest and include:

- randomized path history
- visited nodes
- discovered content
- unlocked reveals
- selected choices
- bookmarks
- current reading position
- score events
- completion percentage

Generated progress must always be clearly identified as synthetic test data.

Synthetic progress must never overwrite real player progress without explicit confirmation.

Synthetic QwestProgress documents should set `runtimeState.synthetic = true`.

Synthetic documents should include metadata such as:

```ts
type SyntheticProgressMetadata = {
  synthetic: true;
  generatedBy: "experienceLab";
  generatedAt: string;
  sourceQwestId: string;
  sourceQwestVersion: string;
  seed?: string;
  scenario?: string;
};
```

Experience Lab may use synthetic progress to test resume, replay, scoring, validation warnings, path reconstruction, map overlays, hidden content, reveal unlocks, and completion states.

### Architecture Principles

Reader runtime owns live session behavior.

Qwest Progress owns persisted progress for one player and one Qwest.

User State owns cross-Qwest aggregation.

Experience Lab may inspect or generate synthetic progress, but it is never the source of truth for player progress.
