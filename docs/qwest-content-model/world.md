# World and Environmental Model

> **Documentation class: Canonical.** This document defines the durable reusable Cast and world-entity contracts plus authored environmental intent and ambience inheritance. It was separated from the [Phase 4 implementation specification](../../story-song-qwest/docs/songqwest-storyqwest-phase-4-unified-content-model-runtime-platorm.md) so later phase status cannot silently redefine the contract.

[Content Model Overview](overview.md) · [Manuscript](manuscript.md) · [Sections](sections.md) · [Runtime Graph](runtime-graph.md) · [Media](media.md) · [Interactions](interactions.md) · [World](world.md) · [Maps](maps.md) · [Journal](journal.md) · [Progress](progress.md) · [Validation](validation.md)

## Cast & World Entities

**Objective**

Introduce reusable world entities that exist independently of any single Qwest while allowing each Qwest to reference and locally customize them.

World entities may be shared across StoryQwests and SongQwests. A single Foundation Cast Member can appear in multiple Qwests with different local roles, descriptions, portraits, visibility, and narrative emphasis without duplicating the underlying identity.

### Goals

- Add first-class Cast Members as reusable Foundation objects.
- Support reusable world entities beyond people.
- Separate Foundation entities from Qwest-specific usage.
- Allow multiple Qwests to reference the same entity.
- Support Qwest-specific overrides without duplicating entities.
- Standardize entity IDs, references, lifecycle, and validation.
- Provide a shared runtime entity viewer.
- Enable interactive text to target any world entity.
- Support future relationship graphs between world entities without redesigning entity IDs or references.
- Preserve runtime compatibility by continuing to publish `Character` objects while using **Cast** terminology throughout Creator surfaces.

### Canonical Entity Model

> **Architecture boundary:** World entities represent identity, not geography. Locations remain defined by the Spatial Content Model (Phase 4I).

World Entity Types:

```text
Cast Member
Organization
Creature
Item
Vehicle
Song
Album
```

Future entity types can be added without redesigning the model.

```ts
type WorldEntityType =
  | "castMember"
  | "organization"
  | "creature"
  | "item"
  | "vehicle"
  | "song"
  | "album";

type WorldEntity = {
  id: string;
  entityType: WorldEntityType;
  name: string;
  description?: string;
  imageId?: string;
  aliases?: string[];
  tags?: string[];
  relationships?: WorldEntityRelationship[];
};
```

Relationships are authored links between world entities. They support future cast viewers, journals, AI context, maps, and discovery surfaces without turning relationship data into runtime flow.

```ts
type WorldRelationshipType =
  | "friend"
  | "enemy"
  | "mentor"
  | "owns"
  | "memberOf"
  | "worksAt"
  | "locatedAt"
  | "related";

type WorldEntityRelationship = {
  id: string;
  type: WorldRelationshipType;
  targetEntityId: string;
  label?: string;
  description?: string;
};
```

Example relationships:

```text
Cory
  -> Works at Workshop
  -> Friend of Bob
  -> Mentor of Alice
  -> Owns Guitar
```

### Foundation Registry

Foundation owns canonical reusable entities.

```text
Foundation
  Cast
  Organizations
  Items
  Creatures
  Vehicles
  Songs
  Albums
```

Foundation entities provide stable IDs, names, baseline descriptions, media references, aliases, tags, and relationships.

The Foundation registry must remain reusable across StoryQwest and SongQwest content. Qwests reference Foundation entities by ID rather than copying full entity records.

### Qwest References & Overrides

Qwests may reference Foundation entities and apply local meaning without changing the Foundation record.

```ts
type QwestEntityReference = {
  id: string;
  entityId: string;
  role?: string;
  localName?: string;
  descriptionOverride?: string;
  imageOverrideId?: string;
  visibility?: "visible" | "hidden" | "discovered";
};
```

```text
Foundation
  Cory

Qwest
  Luthier Impossible
    role: Mentor
  Guitar Repair Tips
    role: Instructor
  Shop Stories
    role: Shop Owner
  Christmas Special
    role: Featured Cast Member
```

Cast terminology belongs in Creator surfaces. Published runtime compatibility may continue to expose `Character` objects, `characters`, and `type: "character"` until the runtime contract is explicitly migrated.

### Validation

The platform validation engine ensures world entities remain reusable and references remain resolvable:

- Entity IDs must be stable and unique within the Foundation registry.
- Qwest entity references must point to valid Foundation entity IDs.
- Qwest overrides must not mutate the Foundation entity record.
- Entity relationships must point to valid world entity IDs.
- Entity relationship types must use an approved `WorldRelationshipType`.
- Relationship records must not create invalid self-references unless explicitly allowed by the relationship type.
- Interactive text targets that reference world entities must resolve to valid Foundation or Qwest entity references.
- Published bundles must preserve runtime compatibility for `Character` objects where required.
- Locations must not be duplicated as world entity types; geographic and spatial references continue to resolve through Phase 4I Location records.

### Acceptance Criteria & Tests

#### Acceptance Criteria

- A Foundation Cast Member can be referenced by multiple StoryQwests and SongQwests without duplicating the entity.
- A Qwest can apply local role, description, portrait, and visibility overrides while preserving the Foundation entity ID.
- World entity relationships can be authored and resolved without affecting runtime flow.
- Interactive text can target a valid world entity.
- Invalid entity IDs, broken relationships, and deleted entity targets are reported by validation.
- Location records remain owned by the Spatial Content Model and are not duplicated as world entities.
- Published bundles remain compatible with existing `Character` runtime consumers where required.

#### Required Tests

- Unit Test: Resolve a Qwest entity reference to its Foundation entity and local overrides.
- Unit Test: Resolve relationships between valid world entities.
- Validation Test: Reject a Qwest entity reference with a missing Foundation entity ID.
- Validation Test: Reject a relationship with an invalid target entity ID.
- Validation Test: Reject a relationship with an unsupported relationship type.
- Validation Test: Reject attempts to model Locations as world entity types.
- Runtime/export Test: Publish compatible `Character` objects for Cast Members while preserving Cast terminology in authoring metadata.
- Interaction Test: Resolve an interactive text target that points to a world entity.

---

---

## Environmental State & Ambience

Phase 4V defines the environment/ambience layer of the Phase 4 world foundation. Phase 4S answers who the reader is looking at through Cast / World Entities. Phase 4T answers where the reader is through Maps / Locations. Phase 4U answers what the reader knows through Journal / Lore / World Knowledge. Phase 4V answers what environmental state the reader is experiencing and what the place, structural section, node, or event feels like.

This phase is documentation-first. It defines the content-model intent and validation direction for ambience without requiring runtime renderer, admin UI, compiler, Firestore rules, or storage changes yet.

### Objective

Environmental State represents the sensory/environmental condition of a Qwest,
structural section node, or event.

Ambience is the reader-facing result of that environmental state.

Ambience may eventually drive:

- Transparent UI overlays.
- Snow.
- Rain.
- Fog.
- Wind.
- Lightning.
- Lighting shifts.
- Particles.
- Subtle motion.
- Ambient audio.
- Haptics.
- Cinematic treatment.

The authored model should express intent, not implementation. A Qwest may say the active environment is `snow`, `storm`, `fog`, `lanternGlow`, or a custom author-defined keyword. It should not configure low-level particle counts, particle velocity, shader settings, opacity curves, renderer internals, transition graphs, or platform-specific effect graphs.

Runtime implementations own how Environmental State is interpreted into reader-facing ambience. The same authored environment keyword may become a transparent overlay in one reader, a simplified static treatment in reduced-motion mode, a lighting shift on another surface, or no visible effect when ambience is disabled by reader preference.

### Why Ambience Comes Before Timeline

Timeline and history modeling depend on a stable world foundation.

The current Phase 4 sequence already establishes:

- Who am I looking at? Cast / World Entities.
- Where am I? Maps / Locations.
- What do I know? Journal / Lore / World Knowledge.

The missing foundational question is:

- What does this place feel like? Ambience.

Ambience should come before Timeline / History because Environmental State describes the reader's present context without requiring chronology, historical causality, flashback modeling, or event-history systems. A structural section can feel like rain, fog, candlelight, stillness, pressure, celebration, or danger before the platform models when those conditions began or where they sit in world history.

Phase 4W can then define Timeline, History, and World Events against a fuller foundation: people, places, knowledge, and environmental state.

### Authoring Model

Authors should be able to:

- Set an overall/default ambience for a Qwest.
- Choose from built-in Environment Keywords.
- Create custom Environment Keywords.
- Control intensity.
- Disable ambience at the structural section or node level.
- Override ambience for specific section nodes, reveals, discoveries, choices,
  or events.

Environment Keywords are author-intent labels. They describe the environment; ambience is what the reader experiences after the runtime interprets that environment.

### Environment Keyword Registry

Phase 4V should document a conceptual Environment Keyword registry without requiring a final runtime effect registry yet.

Built-in Environment Keywords are platform-known labels that may receive richer runtime behavior over time.

Initial built-in keywords may include:

- `rain`
- `snow`
- `fog`
- `wind`
- `storm`
- `fire`
- `embers`
- `dust`
- `magic`
- `underwater`
- `night`
- `sunrise`

Custom Environment Keywords are authored intent labels. They may map to no runtime effect, a generic fallback effect, a future platform effect, or a creator-facing preview hint without invalidating the bundle.

Examples:

- `hauntedOrchard`
- `neonBackstage`
- `emptyCathedral`
- `albumReleaseMidnight`

Unknown built-in keywords should be validation warnings rather than hard failures because custom keywords are allowed. Validation should distinguish an unsupported built-in typo from an intentional custom keyword where possible, but the content model must not prevent authors from naming custom environmental states.

### Trigger Model

Environmental State may be triggered by:

- Qwest start.
- Section-node entry.
- Node entry.
- Keyword match.
- Reveal unlock.
- Discovery activation.
- Choice selection.
- Location change.
- Map region entry.
- Custom event.

Trigger scope describes when the environmental intent becomes active. It should not describe how the runtime paints particles, plays audio, applies lighting, or blends cinematic treatments.

### Inheritance Model

Environmental State resolves from broad author intent to specific moment intent:

```txt
Qwest default environment
  -> Structural section/node environment
  -> Event environment
  -> Runtime produces ambience
```

Multiple runtime implementations may interpret the same Environmental State differently while remaining fully compatible with the published bundle.

Examples:

- A Qwest default ambience is `winter`.
- A structural section node overrides it with `blizzard`.
- Another section node disables ambience during a quiet indoor conversation.
- A reveal temporarily replaces the active ambience with `lightning`.
- After the reveal fade-out, the runtime returns to the inherited section-node
  or Qwest ambience.

The resolved ambience must not alter runtime flow, choices, discoveries, achievements, score, or progress unless a separate explicit effect system later declares state-changing behavior.

### Conceptual Data Model

The following types are conceptual only. They document the intended content shape for future compiler, bundle, validation, and runtime work; they are not a final implementation contract.

```ts
type EnvironmentKeyword = string;

type EnvironmentIntensity = "subtle" | "normal" | "strong" | "cinematic";

type EnvironmentTrigger =
  | "qwestStart"
  | "sectionEnter"
  | "nodeEnter"
  | "keyword"
  | "choiceSelected"
  | "revealUnlocked"
  | "discoveryActivated"
  | "locationChanged"
  | "mapRegionEntered"
  | "customEvent";

type EnvironmentalState = {
  enabled: boolean;
  keyword?: EnvironmentKeyword;
  intensity?: number;
  intensityPreset?: EnvironmentIntensity;
  trigger?: EnvironmentTrigger;
};

type AmbienceRule = {
  id: string;
  environment: EnvironmentalState;
};
```

The content model should store:

- Keyword.
- Enabled/disabled state.
- Intensity.
- Optional trigger scope.

The content model should not store renderer-specific values such as particle count, velocity, shader settings, opacity curves, fade curves, blend graphs, CSS, arbitrary JavaScript, or platform-specific component names.

Richer transition, layering, priority, and blending models may evolve later in the runtime or in a future content-model phase after the author-intent layer is validated.

### Runtime Ownership

The runtime owns rendering and channel decisions.

Runtime may map a resolved `EnvironmentalState` to:

- Transparent UI overlays.
- Visual weather.
- Fog or haze.
- Wind motion.
- Lightning flashes.
- Lighting/tint shifts.
- Particle systems.
- Reduced-motion still treatments.
- Ambient audio after explicit reader opt-in.
- Haptic patterns where supported and enabled.
- Cinematic treatment where supported.

Runtime must also apply reader preferences, device capability, accessibility settings, performance limits, and platform policy. Reader controls such as reduced motion, high contrast, disabled ambience, disabled audio, or disabled haptics override authored environmental intent.

Ambient-audio asset identity and playback requirements are defined by the
canonical [Qwest Media Model](media.md#ambient-audio).

### Relationship To `ambientAssist`

Existing `ambientAssist` fields are retained.

For Phase 4V documentation, `ambientAssist` should be treated as legacy/prototype support and a narrow assistive hint. It can continue to disable or constrain ambience in current bundle shapes, but it is not the full long-term Ambience model.

Future migration may fold `ambientAssist` into the broader Phase 4V Ambience model by mapping:

- `ambientAssist.enabled: false` to `EnvironmentalState.enabled: false`.
- Existing keyword assist behavior to keyword-triggered `AmbienceRule` records.
- Existing effect references to runtime-resolved Environment Keywords or registry-backed effect mappings.

Do not remove `ambientAssist` yet. Existing bundles, prototype runtime behavior, and authoring assistance should remain compatible until a dedicated migration phase defines the replacement path.

### Bundle And Validation Notes

Future bundle validation should follow these rules:

- Unknown built-in environment keywords are warnings, not hard failures, because custom keywords are allowed.
- Invalid intensity ranges are errors.
- Disabled ambience does not require a keyword.
- Enabled ambience should include a keyword unless it only exists to clear, inherit, or disable an inherited environmental state.
- Event-triggered ambience should reference valid node, reveal, discovery, choice, location, or map region IDs where applicable.
- Duplicate ambience rule IDs are errors.
- Ambience rules must not contain executable code, raw CSS, shader code, or renderer-specific configuration.
- Ambience must not become an implicit story-state, unlock, flag, choice, ending, scoring, achievement, discovery, or progress mechanic.

Validation should report ambience warnings and errors against the authored Qwest, section node, event, reveal, discovery, choice, location, or map region where the ambience intent was declared.

### Non-Goals

Phase 4V does not require:

- A final particle renderer.
- A final audio engine.
- A final shader system.
- A final ambience marketplace.
- Timeline/history modeling.
- Inventory systems.
- Quest systems.
- Runtime or admin implementation.
- Firestore rules changes.
- UI components.
- Compiler behavior changes.
