# Maps and Spatial Narrative

> **Documentation class: Canonical.** This document defines the durable spatial records, Journey Maps, authored maps, navigation, exploration, and replay overlays. It was separated from the [Phase 4 implementation specification](../../story-song-qwest/docs/songqwest-storyqwest-phase-4-unified-content-model-runtime-platorm.md) so later phase status cannot silently redefine the contract.

[Content Model Overview](overview.md) · [Manuscript](manuscript.md) · [Sections](sections.md) · [Runtime Graph](runtime-graph.md) · [Media](media.md) · [Interactions](interactions.md) · [World](world.md) · [Maps](maps.md) · [Journal](journal.md) · [Progress](progress.md) · [Validation](validation.md)

## Spatial Narrative Model

Spatial narrative data is an optional layer on top of the canonical node graph.

It exists to support future map-based experiences without redesigning the content model.

Phase 4 defines the contract for spatial narrative data. It does not require runtime map rendering.

```ts
type MapPosition = {
  x: number;
  y: number;
};

type Region = {
  id: string;
  name: string;
  imageId?: string;
};

type Location = {
  id: string;
  name: string;
  regionId?: string;
  position?: MapPosition;
  description?: string;
};
```

Regions represent broad authored spaces. Regions may support hierarchy in future versions.

Examples:

- A StoryQwest world map
- A city district
- An album era
- A tour route
- A game board area

Locations represent named points within the narrative.

Examples:

- Scene locations
- Hidden clue locations
- Song milestones
- Chapter destinations
- World-map points of interest

Nodes may reference locations:

```ts
locationId?: string;
```

Nodes may also contain authored positioning:

```ts
mapPosition?: MapPosition;
```

Spatial position is intentionally abstract. `x` and `y` represent coordinates within an authored map surface, not latitude and longitude.

Future location-aware experiences may add separate geographic metadata without changing the canonical node graph.

### Journey Maps

Journey Maps are a first-class platform concept, not only a UI feature.

Journey Maps are generated from the canonical node graph.

They may consume:

- Runtime flow
- Branches
- Choices
- Reveals
- Discoveries
- Hidden nodes
- Endings
- Authored node positions
- Node location references
- Regions and locations
- Reader-state paths

Journey Maps should support:

- StoryQwest world maps
- SongQwest progression maps
- Visual replay paths
- Validation tools
- Authoring diagnostics
- Future map-based runtimes

Runtime map rendering remains optional.

Consumers may render a Journey Map, validate it, generate a replay visualization, or ignore spatial fields entirely.

---

---

## Maps, Navigation & World Exploration

**Objective**

Build upon the Spatial Content Model by introducing reusable map experiences for authors and readers without changing the underlying runtime graph.

### Goals

- Introduce first-class Map objects.
- Support multiple maps per Qwest.
- Support StoryQwest world maps.
- Support SongQwest progression maps.
- Support indoor and outdoor maps.
- Support map layers.
- Support regions and location grouping.
- Support interactive map hotspots.
- Support runtime exploration.
- Support discovered vs undiscovered locations.
- Support Journey Map rendering.
- Support replay visualization.
- Support future navigation experiences.
- Support map preview and validation in the Experience Lab.
- **Keep map presentation separate from the underlying spatial model defined in Phase 4I.**

### Map Model

Maps are presentation and exploration surfaces layered over the canonical spatial content model. They may reference regions, locations, nodes, media, discoveries, achievements, and Journey Map paths, but they do not replace those underlying records.

```ts
type MapType =
  | "world"
  | "region"
  | "city"
  | "building"
  | "scene"
  | "journey"
  | "progression";

type QwestMap = {
  id: string;
  title: string;
  mapType: MapType;
  regionIds?: string[];
  locationIds?: string[];
  layers?: MapLayer[];
  hotspots?: MapHotspot[];
};

type MapLayer = {
  id: string;
  title: string;
  layerType:
    | "base"
    | "overlay"
    | "journey"
    | "replay"
    | "discovery"
    | "achievement";
  visibleByDefault?: boolean;
};

type MapHotspot = {
  id: string;
  targetId: string;
  targetType:
    | "location"
    | "node"
    | "media"
    | "discovery"
    | "achievement"
    | "worldEntity";
  mapPosition: MapPosition;
  layerId?: string;
  visibility?: "visible" | "hidden" | "discovered";
};
```

Initial Map Types:

```text
World Map
Region Map
City Map
Building Map
Scene Map
Journey Map
Progression Map
```

### Authoring

Authors can create maps that reference existing spatial and narrative records. Map authoring should remain visual and should not require manual ID entry.

Authoring should support:

- Selecting existing regions and locations.
- Placing hotspots on an authored map surface.
- Assigning hotspots to layers.
- Previewing discovered and undiscovered states.
- Previewing Journey Map and replay overlays.
- Validating map hotspots and layer configuration in the Experience Lab.
- Using a future Journey Map Builder to move from imported bundle data into Journey Map, World Map, validation, and replay views.

Example Experience Lab workflow:

```text
Import Bundle
  -> Journey Map Builder
  -> Journey Map
  -> World Map
  -> Validation
  -> Replay
```

### Runtime

Runtime map rendering remains optional unless a specific runtime surface declares support for it.

Runtime Exploration Features:

- Current location
- Visited locations
- Hidden locations
- Map discoveries
- Cast Member locations
- Media hotspots
- Replay paths
- Branch overlays
- Achievement overlays
- Discovery overlays

> **Architecture boundary:** Map presentation must not alter runtime flow, choices, discoveries, achievements, scoring, or reader progress.

Runtime systems derive map state from canonical reader state, node flow, spatial records, and authored map configuration.

### Validation

The platform validation engine ensures map content remains consistent with the canonical spatial and runtime graph models:

- Map IDs must be stable and unique within a Qwest.
- Map region references must resolve to valid Phase 4I Region records.
- Map location references must resolve to valid Phase 4I Location records.
- Hotspot targets must resolve to valid nodes, locations, media, discoveries, achievements, or world entities.
- Hotspot `mapPosition` values must be valid for the authored map surface.
- Layer IDs referenced by hotspots must exist on the same map.
- Journey Map overlays must derive from the canonical node graph.
- Replay overlays must derive from reader-state paths.
- Map presentation data must not redefine runtime graph flow or spatial source records.

### Acceptance Criteria & Tests

#### Acceptance Criteria

- A Qwest can define multiple maps without changing the canonical runtime graph.
- A map can reference existing regions and locations from Phase 4I.
- A map can define layers and hotspots that resolve to valid content targets.
- Experience Lab can preview map layers, hotspots, Journey Map overlays, and validation warnings.
- Runtime exploration features can use map data without requiring every Qwest to render maps.
- Invalid map references, missing hotspot targets, and broken layer references are reported by validation.
- Map presentation remains separate from the underlying spatial model defined in Phase 4I.

#### Required Tests

- Unit Test: Resolve a map region and location reference to Phase 4I spatial records.
- Unit Test: Resolve a hotspot target to a valid node, location, media asset, discovery, achievement, or world entity.
- Validation Test: Reject a map with a missing region or location reference.
- Validation Test: Reject a hotspot that references a missing target.
- Validation Test: Reject a hotspot that references a missing layer.
- Experience Lab Test: Preview map layers, hotspots, Journey Map overlays, and validation warnings.
- Runtime/export Test: Published map data does not alter runtime graph flow or spatial source records.
