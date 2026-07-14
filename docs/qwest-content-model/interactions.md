# Interactions, Discoveries, and Scoring

> **Documentation class: Canonical.** This document defines the durable interactive content, hidden content, reveal, discovery, achievement, scoring, and reader activation contracts. It was separated from the [Phase 4 implementation specification](../../story-song-qwest/docs/songqwest-storyqwest-phase-4-unified-content-model-runtime-platorm.md) so later phase status cannot silently redefine the contract.

[Content Model Overview](overview.md) · [Manuscript](manuscript.md) · [Sections](sections.md) · [Runtime Graph](runtime-graph.md) · [Media](media.md) · [Interactions](interactions.md) · [World](world.md) · [Maps](maps.md) · [Journal](journal.md) · [Progress](progress.md) · [Validation](validation.md)

## Hidden Nodes

Hidden Nodes exist in the narrative graph but are not initially visible.

Examples:

- Secret scenes
- Bonus songs
- Easter eggs
- Unlockable chapters
- Alternate endings

```ts
type HiddenNode = {
  visibility: "hidden";
  conditions: Condition[];
};
```

Supported conditions:

- flags
- unlocks
- discoveries
- achievements
- visited nodes
- choices
- endings

---

## Reveal System

### Definition

A Reveal exposes previously hidden content.

A Reveal is not collectible.

A Reveal changes visibility.

Examples:

- Reveal hidden text
- Reveal hidden media
- Reveal hidden branch
- Reveal hidden discovery
- Reveal hidden ending

```ts
type RevealNode = {
  targetNodeIds: string[];
  conditions: Condition[];
  transition?: Transition;
};
```

#### Reveal Characteristics

- Runtime visibility event
- Usually repeatable
- Usually not tracked permanently
- Presentation-oriented

---

## Discovery System

### Definition

A Discovery is a collectible accomplishment.

Discoveries are permanently recorded.

Examples:

- Hidden logo found
- Secret song discovered
- Bonus scene uncovered
- Rare ending reached

```ts
type Discovery = {
  id: string;
  title: string;
  description?: string;
  points?: number;
};
```

#### Discovery Characteristics

- Permanent
- Tracked
- Replay-oriented
- Completion-oriented

Discoveries may unlock:

- Reveals
- Achievements
- Hidden nodes
- Endings

Phase 4 keeps discoveries in scope because discoveries are content-model concepts.

Phase 4 defines:

- Discovery schema
- Discovery unlock behavior
- Discovery point values
- Discovery reader-state tracking

Presentation, dashboards, celebration UI, and progression displays belong to later phases.

---

## Achievement System

Achievements represent meta-progress.

Achievements are distinct from Discoveries.

Example:

Discovery:

- Hidden Blueprint Found

Achievement:

- Master Explorer
- Find every blueprint

```ts
type Achievement = {
  id: string;
  title: string;
  description: string;
  conditions: AchievementCondition[];
  points?: number;
};
```

Achievements may be:

- Hidden
- Visible
- Progressive

Phase 4 keeps achievements in scope because achievements are content-model concepts.

Phase 4 defines:

- Achievement schema
- Achievement conditions
- Achievement reader-state tracking
- Achievement point values

Presentation, dashboards, celebration UI, and progression displays belong to later phases.

---

## Points & Scoring

Phase 4 introduces optional scoring.

Scoring is not RPG progression.

Scoring exists for:

- Completion
- Discovery tracking
- Replay value
- SongQwest achievements
- StoryQwest achievements

Scoring is event-sourced at the runtime progress layer.

Score totals are derived from `ScoreEvent[]`.

They should not be manually edited as independent canonical values. Runtime implementations may cache total score, discovery count, achievement count, and completion percent for fast display, but those summaries must be recalculable from the event history and current content contract.

```ts
type ScoreEvent = {
  id: string;
  qwestId: string;
  qwestVersion: string;
  playerId?: string;
  type:
    | "textRunCompleted"
    | "textRunReread"
    | "choiceMade"
    | "discoveryFound"
    | "revealUnlocked"
    | "achievementEarned"
    | "endingReached"
    | "hiddenNodeReached"
    | "journalFirstRead";
  points: number;
  sourceType:
    | "textRun"
    | "choice"
    | "discovery"
    | "reveal"
    | "achievement"
    | "ending"
    | "node"
    | "journal";
  sourceId: string;
  occurrenceKey?: string;
  repeatable: boolean;
  wordCount?: number;
  awardedAt: string;
  metadata?: Record<string, unknown>;
};
```

Examples:

- Discovery found
- Achievement earned
- Ending reached
- Secret uncovered
- Text run completed
- Text run reread
- Choice made
- Reveal unlocked
- First journal read

Reader state may include:

```ts
scoreSummary: {
  totalScore: number;
  discoveryCount: number;
  achievementCount: number;
  revealCount: number;
  endingCount: number;
  completionPercent?: number;
};
scoreEvents: ScoreEvent[];
```

### Repeatability Rules

Repeatable events may be awarded more than once when the runtime records a distinct occurrence.

Repeatable platform scoring events include:

- text-run completion
- text-run reread
- choices made

Awarded-once events may only be awarded once per player, Qwest, Qwest version boundary, and source ID unless an explicit migration or replay policy says otherwise.

Awarded-once platform scoring events include:

- discoveries
- reveals
- achievements
- endings
- hidden nodes
- first journal read

For awarded-once events, the runtime must use a stable de-duplication key such as:

```ts
`${qwestId}:${qwestVersion}:${type}:${sourceId}`;
```

Text scoring should be recorded at text-run completion granularity rather than as one event per word. A completed `TextRun` with `wordCount = 86` may award 86 points, and a reread of that same text run may award another repeatable 86-point event. This keeps the same scoring behavior while avoiding unbounded event growth for long or frequently reread Qwests.

Repeatable events may include an `occurrenceKey` for deterministic replay, such as a completed text-run read session, branch traversal, or choice interaction timestamp bucket.

Phase 4 keeps points and scoring in scope because they are content-model concepts.

Phase 4 defines:

- Point schema
- Score event schema
- Discovery scoring
- Achievement scoring
- Reader-state tracking
- Repeatable and awarded-once scoring semantics
- Deterministic score recalculation from score event history

Presentation, dashboards, celebration UI, and progression displays belong to later phases.

---

---

## Interactive Text & Interactive Elements

Interactive Elements define how authored text becomes a runtime interface.

The platform should not maintain separate systems for reveals, media, cast member lookups, location lookups, journal entries, external links, or discoveries. They all use one interaction model.

Interactive text is part of authored content, not presentation.

Reveal remains a content type.

Discovery remains a content type.

Media remains a content type.

Interactive Element describes how the reader activates one content target from text or another runtime surface.

The text itself becomes the interface into the world.

### Authoring Model, Compiler, Runtime Model

The interaction architecture has a strict boundary:

```txt
Authoring Model
Compiler
Runtime Model
```

The authoring model stores stable rich text structure.

The compiler transforms authored rich text into the published runtime contract.

The runtime model consumes compiled `InlineInteraction` objects and content targets.

Authoring and runtime representations do not need to be identical.

The canonical authoring model must not depend on character offsets. Offsets become fragile as soon as an author edits a paragraph. If a runtime needs offsets, the compiler may calculate them for the published bundle.

### Interaction Scope

Interactions may exist anywhere rich text is rendered.

Supported rich-text surfaces include:

- Paragraph text
- Heading text
- Choice labels
- Image captions
- Discovery descriptions
- Journal entries
- Cast biographies
- Location descriptions
- Section summaries
- Future rich-text content surfaces

### Content Targets

Interactive Elements point to content targets.

```txt
Interactive Text
Content Target
Runtime View
```

This keeps the interaction model independent from individual content types and prepares the platform for future extension.

Every addressable node may be exposed as a ContentTarget, but not every ContentTarget is a runtime node.

World entities, locations, maps, journal entries, media assets, external URLs, and commands can be content targets without becoming nodes in the runtime story graph.

```ts
type ContentTargetType =
  | "node"
  | "media"
  | "reveal"
  | "discovery"
  | "worldEntity"
  | "character"
  | "location"
  | "map"
  | "journal"
  | "ending"
  | "external"
  | "command";

type ContentTargetRef = {
  id: string;
  type: ContentTargetType;
};

type ContentTarget = {
  id: string;
  type: ContentTargetType;
  title?: string;
  description?: string;
  sourceId?: string;
  externalUrl?: string;
  command?: ContentCommand;
  presentationHint?: ContentPresentationHint;
};

type ContentCommand = "openInventory" | "openMap" | "playSong" | "showCredits";

type ContentPresentationHint =
  | "default"
  | "dialog"
  | "drawer"
  | "sidePanel"
  | "fullscreen"
  | "popover"
  | "inline";
```

`presentationHint` is optional content metadata, not a renderer selection.

Content must not know how it is rendered. Runtime systems may register renderers internally for each content target type, command, or presentation context.

`command` is reserved for approved platform commands. It does not allow arbitrary JavaScript or author-supplied executable code.

Optional target registries support interaction types that are not themselves runtime-flow nodes.

```ts
type Character = {
  id: string;
  name: string;
  description?: TextRun[];
  imageId?: string;
};

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

`JournalEntry` is the canonical runtime object for Journal/Lore content:

- `journal` is a reader-facing collected entry.
- `lore` is world/background knowledge.
- `note` is short-form discovered text.
- `book` is long-form in-world text.
- `letter` is stylized correspondence.
- `document` is an artifact, record, report, clue, contract, recipe, map note, or other readable object.
- `visibility` controls whether the entry is initially available.
- `discoveryId` optionally links the entry to an existing Discovery so opening or finding the entry can mark a discovery.
- `sourceNodeId` optionally records where the entry was introduced or discovered.

Journal/Lore entries are supplemental by default. They must not silently alter flow, choices, discoveries, achievements, score, or progress unless explicitly connected through existing node, discovery, condition, or choice mechanisms.

### Interaction Types

```ts
type TextInteraction =
  | "none"
  | "media"
  | "reveal"
  | "discovery"
  | "worldEntity"
  | "character"
  | "location"
  | "journal"
  | "external"
  | "command";
```

`worldEntity` is the preferred target type for Cast and other Foundation identity records. `character` remains available as a compatibility target while existing bundles and runtime viewers migrate.

The interaction model must remain extensible so future content types, such as maps, notes, inventory-like objects, timeline entries, and music credits, can be introduced without redesigning text rendering.

### Authoring Rich Text Runs

Paragraphs are not plain authoring strings.

The authoring model stores rich text runs.

```ts
type TextRun = {
  id: string;
  text: string;
  interaction?: AuthoredTextInteraction;
};

type AuthoredTextInteraction = {
  id: string;
  type: TextInteraction;
  target: ContentTargetRef;
  activation?: InteractionActivation;
  indicator?: InteractionIndicator;
  conditions?: Condition[];
};
```

Example authoring markdown:

```md
The [Lantern Dragon]{media:lantern-dragon}
```

Example editor model:

```txt
Paragraph
  Text Run
  Text Run (interactive)
  Text Run
```

Example authoring data:

```ts
{
  text: "the dragon map",
  interaction: {
    id: "interaction-dragon-map",
    type: "media",
    target: {
      id: "dragon-map",
      type: "media"
    },
    activation: "tap",
    indicator: "underline"
  }
}
```

### Runtime Inline Interaction Contract

Runtime interactions should be first-class JSON objects.

They must not exist only as markdown syntax.

```ts
type InlineInteraction = {
  id: string;
  type: TextInteraction;
  target: ContentTargetRef;
  activation: InteractionActivation;
  indicator: InteractionIndicator;
  conditions?: Condition[];
  sourceNodeId: string;
  sourceRunId?: string;
  compiledRange?: {
    startOffset: number;
    endOffset: number;
  };
  presentation?: InteractionPresentationConfig;
  pressHold?: PressHoldConfig;
  analytics?: InteractionAnalyticsConfig;
};
```

`sourceNodeId` identifies the paragraph, heading, section, or node that owns the rich-text run.

`sourceRunId` may identify the stable authored text run that produced the runtime interaction.

`compiledRange` is optional runtime metadata calculated by the compiler. It must not be treated as the canonical authoring model.

`target` references a content target, addressable node, media asset, discovery, cast member, location, journal entry, ending, or external URL.

The manuscript may remain readable, but the compiled bundle must know which text runs are interactive.

### Content Target Lifecycle

Content Targets must have stable IDs.

Content Targets may be renamed.

Renaming a Content Target must never invalidate authored links.

Deleting a Content Target must create a validation error until all references are removed or reassigned.

Moving a Content Target between collections must not affect references.

Publish validation must fail if unresolved Content Targets remain.

Content Target lifecycle rules apply equally to nodes, media, cast members, locations, journal entries, discoveries, endings, external links, and approved commands.

### Interaction Conditions

Interactions may be conditional.

This allows text to become interactive only after the reader has met a cast member, found an object, reached an ending, completed a discovery, visited a location, or satisfied another authored condition.

Example:

```ts
{
  text: "portrait",
  interaction: {
    id: "interaction-fireplace-portrait",
    type: "reveal",
    target: {
      id: "portrait-secret",
      type: "reveal"
    },
    activation: "pressHold",
    indicator: "dottedUnderline",
    conditions: [
      {
        type: "discoveryFound",
        discoveryId: "met-cory"
      }
    ]
  }
}
```

If conditions are not met, the text should render as ordinary text unless the author explicitly chooses a disabled or hinted presentation.

Conditional interactions allow the paragraph to stay stable while the interactive layer changes with reader state.

### Interaction Ownership Rules

A text range may own only one Interactive Element.

Nested interactions are not allowed.

Overlapping interactions are not allowed.

Example invalid authoring shape:

```txt
The [dragon [map]](...)
```

If a text range appears to have more than one possible action, the author must choose one interaction target or split the text into separate non-overlapping runs.

This resolves priority conflicts between links, reveals, discoveries, and other interactive behavior.

### Reader Flow Principle

Interactive Elements never advance narrative progression unless explicitly authored to do so.

Their default behavior is to temporarily suspend the reading experience, present supplemental content, and return the reader to the same reading position upon dismissal.

An interaction may affect progression only when its target, conditions, or flow behavior explicitly declare that outcome.

### Activation Behavior

Activation is configurable.

```ts
type InteractionActivation =
  | "tap"
  | "doubleTap"
  | "pressHold"
  | "hover"
  | "automatic";
```

Default behavior:

- Media defaults to tap.
- Reveal defaults to press and hold.
- Discovery defaults to press and hold when hidden, tap when visibly indicated.
- Cast member, location, and journal interactions default to tap.
- External links default to tap.

Authors may override the default where accessible.

Hover must only be an enhancement. It cannot be the only activation path.

### Press-and-Hold Configuration

Press-and-hold interactions should feel intentional and inspectable.

```ts
type PressHoldConfig = {
  durationMs: number;
  animation?: "none" | "fill" | "ring" | "glow" | "pulse";
  soundId?: string;
  cancelThresholdPx?: number;
  progressIndicator?: "none" | "bar" | "ring" | "inline";
  label?: string;
};
```

Example reader copy:

```txt
Hold to Inspect
```

Press-and-hold progress should be cancellable when the reader moves outside the configured threshold.

Reduced-motion preferences should disable or simplify hold animations.

### Visual Indicators

Authors control how obvious an interaction is.

```ts
type InteractionIndicator =
  | "none"
  | "underline"
  | "highlight"
  | "icon"
  | "glow"
  | "dottedUnderline"
  | "custom";
```

Some interactions should be obvious.

Some should be discoverable only through careful reading.

The runtime must support subtle interactions without making them inaccessible.

Authoring preview must show inline indicators even when the reader-facing indicator is subtle or hidden.

### Shared Presentation / Viewer Behavior

Interactive Elements do not prescribe presentation.

The default presentation is a modal dialog, but runtimes may render the interaction as a drawer, side panel, fullscreen view, popover, split view, inline expansion, or other appropriate presentation without changing the authored content.

Interactive Elements should use a common viewer framework where possible.

Different interaction types may provide different content, but the dismissal and accessibility model should remain consistent.

```ts
type InteractionPresentationConfig = {
  presentation?:
    | "dialog"
    | "drawer"
    | "sidePanel"
    | "fullscreen"
    | "popover"
    | "splitView"
    | "inline";
  dismiss: ("swipe" | "tapOutside" | "closeButton" | "escape" | "backButton")[];
};
```

Runtime should support:

- Swipe dismissal where appropriate.
- Tap outside dismissal where appropriate.
- Close button dismissal.
- Escape key dismissal.
- Browser or device back button dismissal.

Close button and keyboard dismissal must always be available for accessible presented experiences.

### Reader Interaction State

Reader state should track interaction status without inventing a separate system for each interaction type.

```ts
type InteractionStatus = "neverOpened" | "opened" | "read" | "completed";

type InteractionState = {
  interactionId: string;
  status: InteractionStatus;
  openedAt?: string;
  readAt?: string;
  completedAt?: string;
  openCount?: number;
};
```

This enables:

- Inspected / not inspected states.
- Discovery completion.
- Read receipts for journal entries or lore.
- Replay and resume behavior.
- Optional analytics aggregation.

### Interaction Analytics Hooks

Analytics are not required for MVP.

The content model should still allow future instrumentation without changing the interaction contract.

Potential signals:

- Inspection count.
- Average hold time.
- Never found.
- Frequently found.
- Dismissed immediately.
- Completed.

```ts
type InteractionAnalyticsConfig = {
  enabled?: boolean;
  eventName?: string;
};
```

Runtime analytics must respect privacy settings and product analytics policy.

### Accessibility

Press and hold cannot be the only way to activate important content.

Accessibility mode may transform activation behavior.

Example:

```txt
pressHold -> tap
```

Requirements:

- All interactions must be keyboard reachable when rendered on keyboard-capable devices.
- All interactive text must expose an accessible name and role.
- Press-and-hold interactions must provide an alternate activation.
- Hover interactions must provide tap or keyboard activation.
- Reduced-motion preferences must simplify transitions and hold animations.
- Screen readers must announce when text has interactive content.
- Reader preferences may override activation behavior globally.

### Rich Text Authoring

Authors should not manually enter IDs.

Authors should not need markdown syntax for inline interactions.

The authoring flow should be visual:

```txt
Highlight text
Right click or open inline menu
Link To...
```

The first action should be a universal picker, not a type-first workflow.

Example picker:

```txt
Link To...

Search...
Recent

Sections
Media
Cast
Locations
Journal
Reveals
Discoveries
Endings
External URL
Commands

Create New...
```

The author should think, "I want this text to open Cory's biography," not, "I am creating a reveal interaction."

The editor infers the interaction type from the selected content target.

`Create New...` is not required for the initial Phase 4 runtime, but the architecture should reserve it as a first-class authoring workflow.

When an author highlights text and chooses `Create New...`, the editor may open a focused creation dialog for media, cast member, location, journal entry, discovery, reveal, or another supported content type. After saving, the editor should create the content target and immediately link the selected text without forcing the author to leave the manuscript.

Type-specific authoring labels may still appear where helpful.

```txt
Reveal
Media
Cast member
Location
Journal Entry
Discovery
Heading
Paragraph
Choice
Section
External URL
Remove Link
```

The platform model should store these as Interactive Elements.

The manuscript compiler owns the conversion from authored rich text to markdown and compiled JSON.

### Inline Author Preview

Author preview should make interactivity visible.

Examples:

```txt
The fireplace [inspect icon]
```

```txt
The fireplace
-------------
```

Preview indicators should show:

- Interaction type.
- Target status.
- Activation behavior.
- Broken target warnings.
- Reader-facing indicator choice.

Authors should be able to inspect and remove an interaction from the same inline preview surface.

### Interaction Validation

Validation should detect:

- Broken interaction targets.
- Circular reveal interactions.
- Duplicate interactive regions.
- Overlapping highlighted text.
- Orphaned media.
- Unreachable reveals.
- Unreachable discoveries.
- Invalid activation type.
- Invalid indicator type.
- Invalid interaction conditions.
- Interaction conditions that reference missing flags, discoveries, achievements, endings, nodes, locations, or content targets.
- External interactions without valid URLs.
- Press-and-hold interactions without an accessible fallback.
- Nested interactions.
- Multiple interactions on the same text range.
- Runtime compiled ranges that no longer match source text.
- Deleted or unresolved content targets.
- Content target references broken by delete, move, or reassignment.

Validation should report interaction errors against the authored text range whenever possible.
