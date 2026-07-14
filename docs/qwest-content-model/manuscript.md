# Manuscript Model

> **Documentation class: Canonical.** This document defines the durable relationship between authored manuscript structure, runtime flow, addressable content, transitions, and choices. It was separated from the [Phase 4 implementation specification](../../story-song-qwest/docs/songqwest-storyqwest-phase-4-unified-content-model-runtime-platorm.md) so later phase status cannot silently redefine the contract.

[Content Model Overview](overview.md) · [Manuscript](manuscript.md) · [Sections](sections.md) · [Runtime Graph](runtime-graph.md) · [Media](media.md) · [Interactions](interactions.md) · [World](world.md) · [Maps](maps.md) · [Journal](journal.md) · [Progress](progress.md) · [Validation](validation.md)

## Manuscript Structure vs Runtime Flow

These are separate concerns.

### Document Order

Controls:

- Author manuscript layout
- Outline structure
- Author navigation
- Preview positioning

```ts
documentOrder: number;
```

### Runtime Flow

Controls:

- Reader progression
- Branching
- Choices
- Reveals
- Continuations
- Endings

```ts
flow: FlowTarget[];
```

Document order must never determine runtime progression.

---

## Unified Node System

Everything becomes a node.

```ts
type NodeType =
  | "section"
  | "choiceHub"
  | "branch"
  | "mediaMoment"
  | "reveal"
  | "discovery"
  | "ending"
  | "hiddenNode"
  | "scriptContainer";
```

```ts
type SectionType = "scene" | "song" | "dialogue" | "journal" | "letter";
```

```ts
type SectionRole = "prologue" | "epilogue" | "credits";
```

- All nodes are addressable.
- All nodes can participate in validation.
- All nodes can be linked or targeted.

`nodeType` owns runtime graph behavior. `sectionType` describes the semantic
content contained by a section. `sectionRole` describes the section's structural
placement within the overall work. `sectionType` and `sectionRole` are valid only
when `nodeType` is `"section"`; neither field creates graph edges, changes
reachability, or introduces a new runtime node behavior.

When `sectionType` is omitted, the node is a general narrative section. When
`sectionRole` is omitted, the section belongs to the main body of the work.

Every structural Manuscript Section compiles into exactly one canonical QwestNode. Elements like paragraphs and headings become rich text runs contained inside a structural Section node rather than cluttering the runtime graph layout.

```ts
type QwestNode = {
  id: string;
  title?: string;
  nodeType: NodeType;
  sectionType?: SectionType;
  sectionRole?: SectionRole;
  documentOrder: number;
  flow: FlowTarget[];
  textRuns?: TextRun[];
  locationId?: string;
  choices?: Choice[];
  icon?: string;
  mapPosition?: MapPosition;
  ambientAssist?: AmbientAssistOverride;
};
```

A general authored Section compiles as `nodeType: "section"` with
`sectionType` omitted. `scene`, `song`, `dialogue`, `journal`, and `letter` are
authored semantic varieties of that same runtime section behavior. For example,
an authored Scene compiles as `nodeType: "section"` and
`sectionType: "scene"`; a Song compiles as `nodeType: "section"` and
`sectionType: "song"`.

`branch` is a real runtime node because it has content, title, document order, validation, previewability, and its own continuation flow.

Choices are edges/actions from a `choiceHub` to target branch nodes. They are not standalone nodes unless future behavior requires independent runtime identity.

Example graph:

```text
Section
  -> ChoiceHub
      choice -> Branch
                  -> Section or Ending
      choice -> Branch
                  -> Section or Ending
```

`title` is included as an optional property on structural nodes (`section`, `branch`, `ending`, `hiddenNode`) to preserve the author's structural context (e.g., Chapter names, Scene titles) down into the runtime bundle, matching the manuscript outline.

`locationId` links a node to an authored location.

`mapPosition` allows a creator or tool to place a node directly on a map when a location record is unnecessary.

Choice Hub nodes and section-ending decisions may define `choices[]`. Each choice carries reader-facing decision metadata and a target. The compiler and runtime still derive graph traversal from valid targets so Journey Maps, validation, and replay remain consistent.

Spatial fields must never be required for ordinary reading, preview, publishing, or validation.

Media identity, references, narrative roles, moments, playback intent, and
accessibility are defined by the canonical [Qwest Media Model](media.md).

---

## Transition System

Phase 4 defines the transition contract and schema.

Transitions remain in Phase 4 because transition names and configuration are part of the content model.

Transitions create visual feedback when:

- Reveals occur
- Discoveries occur
- Media appears
- Endings trigger
- Achievements unlock

Supported transitions:

```ts
type TransitionType =
  | "none"
  | "fade"
  | "fadeIn"
  | "fadeOut"
  | "slideUp"
  | "slideDown"
  | "zoom"
  | "pulse"
  | "sparkle"
  | "confetti"
  | "explosion"
  | "glow"
  | "flash";

type Transition = {
  type: TransitionType;
  durationMs?: number;
  easing?: string;
  variant?: string;
};
```

Example:

```ts
transition: {
  type: "confetti";
  durationMs: 1500;
}
```

Transitions are selected by creators.

Runtime implements them consistently.

Visual implementation and polish may occur in later phases.

The transition model must remain extensible so future developers can add additional transition types without changing the content model.

---

## Continuation Model

Continuation becomes explicit.

```ts
type Continuation =
  | "next"
  | "choiceRequired"
  | "target"
  | "ending"
  | "terminal";
```

This eliminates accidental dead ends.

Nodes must intentionally declare terminal behavior.

Validation should detect unintended terminals.

---

## Addressable Targets

Any addressable node may be targeted.

Supported targets:

- section
- choiceHub
- branch
- heading
- paragraph
- reveal
- discovery
- mediaMoment
- ending
- hiddenNode
- scriptContainer

Interactive text may additionally target:

- media asset
- character
- location
- journal entry
- external URL

Authors should not manually enter IDs.

Selection should remain visual.

Locations and regions use the same visual selection pattern for spatial references, but they do not become runtime flow targets unless represented by a node.

---

---

## Choice Model & Reader Decision Metadata

Phase 4R establishes Choices as first-class narrative objects containing both structural flow targets and optional reader-facing presentation metadata. This explicitly separates narrative meaning (the content model) from visual expression (the design system).

### Canonical Choice Schema

Choices are compiled directly into the runtime graph as rich metadata structures rather than overloading structural `FlowTarget` nodes.

Choice metadata enriches reader decisions but never replaces the canonical runtime graph. Branch validation, Journey Maps, replay, and analytics continue to operate on graph targets rather than presentation metadata.

```ts
type Choice = {
  id: string;
  title: string;
  description?: string;
  prompt?: string;
  targetNodeId: string;
  kind?: ChoiceKind;
  requirements?: ChoiceRequirement[];
  consequenceHint?: ChoiceConsequenceHint;
  presentation?: ChoicePresentationHint;
  availability?: ChoiceAvailability;
  scriptContainerId?: string;
  lockedReason?: string;
};
```

prompt provides optional narrative framing that appears above one or more choices. It is not part of the choice itself and does not affect branching logic.

Presentation hints are advisory metadata. Runtime implementations may ignore them when another presentation better suits the current device, accessibility settings, or reader mode.

Description provides optional narrative context for an individual choice without revealing its outcome.

### Classification & Framing Metadata

Choice Kinds
Defines the narrative intent of a decision space. The design system translates these categories into specific card variations or layout styles.

```ts
type ChoiceKind = "continue" | "decision" | "discovery" | "script" | "ending";
```

Reader Consequence Hints
Communicates tone and psychological weight to the reader without revealing explicit outcomes or branching logic.

```ts
type ChoicePresentationHint =
  | "default"
  | "literary"
  | "quiet"
  | "scriptLaunch"
  | "continue";
```

```ts
type ChoiceConsequenceHint =
  | "unknown"
  | "relationship"
  | "danger"
  | "exploration"
  | "reward";
```

### Reader Context & Requirements

Requirements
Enforces gating mechanics based on the reader's persistent world state or interactive discoveries.

```ts
type ChoiceRequirementType =
  | "discovery"
  | "achievement"
  | "flag"
  | "scriptResult"
  | "characterTrust";
```

```ts
type ChoiceRequirement = {
  type: ChoiceRequirementType;
  refId: string;
  label?: string;
};
```

```ts
type ChoiceAvailability =
  | "available"
  | "locked"
  | "hidden"
  | "disabled"
  | "completed";
```

### Choice Model Validation Rules

The platform validation engine ensures choice logic balances flexibility with technical integrity:

- Presentation Isolation: Rejects any choice configuration supplying raw color codes, font filenames, or pixel boundaries.
- Dangling Requirements: Throws an error if a ChoiceRequirement targets an invalid refId (e.g., an unlisted discovery or non-existent world flag).
- Runtime Graph Soundness: Ensures targetNodeId accurately routes to a valid, reachable destination node within the compiled graph.
