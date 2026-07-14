# Content Model Validation

> **Documentation class: Canonical.** This document defines the durable validation responsibilities and acceptance evidence required across authoring, preview, publishing, and runtime. It was separated from the [Phase 4 implementation specification](../../story-song-qwest/docs/songqwest-storyqwest-phase-4-unified-content-model-runtime-platorm.md) so later phase status cannot silently redefine the contract.

[Content Model Overview](overview.md) · [Manuscript](manuscript.md) · [Sections](sections.md) · [Runtime Graph](runtime-graph.md) · [Media](media.md) · [Interactions](interactions.md) · [World](world.md) · [Maps](maps.md) · [Journal](journal.md) · [Progress](progress.md) · [Validation](validation.md)

## Story Map Evolution

Story Map should visualize:

- Branches
- Reveals
- Discoveries
- Hidden nodes
- Achievements
- Endings
- Script containers
- Locations
- Regions
- Spatial relationships
- Journey paths
- Map overlays
- Replay paths

Future graph analysis should operate on the unified node graph.

Story Maps should evolve from simple branch visualization into graph-aware spatial tools.

They may consume canonical node flow, authored positions, location references, region definitions, and reader-state paths.

Story Map overlays may include validation warnings, hidden content, discovery routes, achievement progress, replay paths, and future map-specific annotations.

Story Map evolution should not require all Qwests to define spatial data.

---

## Validation Evolution

Validation should detect:

- Dead ends
- Unreachable nodes
- Unreachable discoveries
- Unreachable achievements
- Unreachable reveals
- Unreachable endings
- Hidden-node cycles
- Invalid transitions
- Invalid location references
- Invalid region references
- Invalid spatial positions
- Missing media
- Broken interaction targets
- Circular reveal interactions
- Duplicate interactive regions
- Overlapping interactive text ranges
- Orphaned interaction targets
- Deleted or unresolved content targets
- Content target references broken by delete, move, or reassignment
- Invalid activation types
- Invalid indicator types
- Runtime compiled interaction ranges that no longer match source text
- Nested interactions
- Multiple interactions on the same text range
- Invalid script configuration
- Runtime service definitions that bypass the service, manager, registry, module, and resolver organization pattern.
- Runtime code paths that resolve IDs through ad hoc array searches where a resolver should own the lookup.
- Resolver results that do not return explicit success, fallback, or error state.
- Invalid platform registry reference.
- Registry references that point to deprecated entries without an approved fallback.
- Registry entries that omit required capability or fallback metadata.
- Registry entries with invalid or contradictory requirements metadata.
- Registry-backed extension points that attempt to operate as arbitrary third-party plugins.
- Invalid reader theme ID.
- Invalid reader theme font, surface, block, spacing, scale, or accent token.
- Reader theme values outside approved ranges.
- Reader theme values that attempt raw CSS, external font URLs, script injection, or app-shell styling.
- Invalid ambient effect ID.
- Ambient rules that reference unregistered effect IDs.
- Ambient rules with invalid triggers.
- Ambient keyword rules without safe word-boundary matching.
- Ambient tags that reference missing nodes or text runs.
- Per-node ambient overrides that reference missing rules or effects.
- Ambient zone references that point to missing zones.
- Ambient zones that reference missing start or end nodes.
- Ambient zones that reference unregistered effect IDs.
- Ambient transitions that reference unregistered source or target effects.
- Ambient channel preferences with unsupported channel names.
- Ambient module capability declarations that conflict with requested transitions or channels.
- Ambient audio references that do not resolve to approved media assets.
- Ambient content that attempts arbitrary JavaScript, CSS, CodePen embeds, external executable embeds, or author-supplied effect code.
- Environmental states with invalid intensity ranges.
- Disabled environmental states that incorrectly require a keyword.
- Event-triggered environment rules with missing or invalid node, reveal, discovery, choice, location, or map region references where applicable.
- Unknown built-in environment keywords should warn rather than hard fail because custom environment keywords are allowed.
- Artwork references that point to missing media assets.
- Artwork references that point to non-image media assets.
- World entity references that point to missing Foundation entities.
- World entity relationships that point to missing target entities.
- Map references that point to missing regions or locations.
- Map hotspots that point to missing targets or layers.
- Map presentation data that attempts to redefine runtime graph flow or spatial source records.
- Duplicate journal entry IDs.
- Journal entries with invalid `entryType` values.
- Journal entries with invalid `TextRun[]` body content.
- Journal entry media references that point to missing media assets.
- Journal `relatedTargetRefs` that point to missing or invalid content targets.
- Journal `discoveryId` references that point to missing discoveries.
- Journal `sourceNodeId` references that point to missing nodes.
- Interactive text targets of type `journal` that point to missing journal entries.
- Hidden journal entries without a valid unlock, discovery path, or intentional hidden state.
- Deleted journal entries still referenced by interactive text or related target references.
- Journal entries that attempt to alter runtime graph flow without an explicit node, discovery, condition, or choice connection.
- QwestProgress documents with invalid current node references.
- QwestProgress documents with invalid current node or text-run references.
- QwestProgress documents with stale save positions that cannot be mapped to the current Qwest version.
- QwestProgress bookmarks with invalid or deleted addressable targets.
- Duplicate awarded-once score events for the same player, Qwest, Qwest version, event type, and source ID.
- Score summaries that do not match deterministic recalculation from ScoreEvents.
- Orphaned interaction state with no matching interactive text, reveal, discovery, journal, media, or content target.
- Invalid reveal references in progress documents.
- Invalid discovery references in progress documents.
- Corrupted journey paths with impossible node transitions, missing nodes, or location references that no longer resolve.
- Path history entries that reference deleted choices or unreachable target nodes without migration metadata.
- Progress documents with unsupported schema versions and no available migration path.
- Migrated progress documents that fail to preserve valid score events, path history, bookmarks, and resume location where possible.

Validation should remain actionable.

---

## Qwest Progress Acceptance Criteria & Tests

Qwest Progress behavior is successful when:

- A reader can resume from a saved QwestProgress document after closing and reopening the reader.
- Current node, text run, reading position, and scroll offset restore the closest valid runtime location.
- `firstOpenedAt` is set when the progress document is created and `lastOpenedAt` updates when the Qwest is opened again.
- Runtime lifecycle flags are stored in `runtimeState` rather than authored story flags.
- Authored story progression fields are stored under `storyState`.
- Future runtime-owned systems can reserve data under `extensions` without changing the QwestProgress root shape.
- Score totals are derived from ScoreEvents and can be recalculated deterministically.
- Repeatable text-run completion scoring can award multiple valid events without conflicting with awarded-once scoring.
- Text-run reread scoring is recorded separately from first-completion scoring.
- One-time discovery scoring cannot be awarded twice for the same discovery source.
- Reveals, achievements, endings, hidden nodes, and first journal reads use stable awarded-once score keys.
- Bookmarks persist across sessions and resolve to valid addressable targets after reload.
- Invalid bookmark targets are rejected or migrated to safe fallback targets.
- Journey path and path history can reconstruct the reader's route through the Qwest.
- Replay generation can derive a readable traversal from path history, choices made, discoveries, reveals, endings, and score events.
- Synthetic progress generation can produce test progress without overwriting real player progress.
- Synthetic progress is clearly marked as synthetic test data.
- Older progress documents migrate to the current schema without losing valid progress.
- UserState aggregates cross-Qwest totals without duplicating full QwestProgress documents.
- Save slots can be represented as a future layer above QwestProgress without making save slots the source of truth for progress.

Recommended tests:

- Runtime Test: Save progress in the middle of a node, reload the reader, and resume at the same node, text run, and nearest supported reading position.
- Runtime Test: Open a Qwest for the first time, reopen it later, and verify `firstOpenedAt` remains stable while `lastOpenedAt` changes.
- Unit Test: Recalculate total score from ScoreEvents and assert it matches the cached score summary.
- Unit Test: Award text-run completion and text-run reread events from `TextRun.wordCount` and verify each valid occurrence contributes to the score without creating per-word events.
- Unit Test: Attempt to award the same discovery event twice and verify the second event is ignored or rejected.
- Unit Test: Store completion, abandonment, import, replay, and synthetic status in `runtimeState` without mutating authored story flags.
- Unit Test: Store authored flags, unlocks, discoveries, reveals, achievements, and endings in `storyState`.
- Unit Test: Validate future runtime-owned extension data can be present without changing core navigation, storyState, scoring, statistics, or readerContent contracts.
- Unit Test: Persist a bookmark, reload progress, and resolve the bookmark target through the addressable-target resolver.
- Unit Test: Reconstruct path history from choices and node transitions.
- Runtime Test: Generate replay data from a completed QwestProgress document.
- Experience Lab Test: Generate synthetic progress with randomized path history, discovered content, unlocked reveals, selected choices, bookmarks, score events, and completion percentage.
- Migration Test: Load an older ReaderState document and migrate it into QwestProgress with valid resume location, score events, bookmarks, and path history.
- Validation Test: Reject progress documents with invalid node references, invalid bookmark targets, duplicate awarded-once score events, orphaned interaction state, invalid reveal references, invalid discovery references, corrupted journey paths, stale save positions, or unsupported schema versions.

---

## Reader Theme Acceptance Criteria & Tests

Reader Theme behavior is successful when:

- A Qwest with no `readerTheme` uses the default reader theme.
- A Qwest with a valid `readerTheme` changes only story typography and story surface presentation.
- App shell typography and controls do not change when a Qwest theme is applied.
- Reader toolbar, account/auth UI, editor/creator UI, global buttons, and menus keep platform styling.
- Invalid theme IDs fall back safely to the default reader theme.
- Unsafe values, unknown tokens, raw CSS, scripts, and external font URLs are rejected by validation or normalized to safe defaults.
- User reader preferences override author theme values.
- Larger text, reduced motion, high contrast, and disabled ambience/effects remain available for every themed Qwest.
- Theme resolution and presentation work in continuous reader mode.
- Theme resolution and presentation work in paginated reader mode.
- Discovery, reveal, choice, scoring, achievement, and progression logic are unchanged by theme data.

Required tests:

- Unit test: theme resolver returns the default reader theme when `readerTheme` is missing.
- Unit test: theme resolver applies a valid registered theme and valid token overrides.
- Unit test: unknown theme IDs fall back to the default reader theme.
- Unit test: invalid typography, surface, block, spacing, scale, accent, `lineHeight`, and `maxWidth` values are rejected or normalized.
- Validation test: raw CSS, scripts, external font URLs, and arbitrary class names are not accepted in Qwest theme data.
- Runtime test: applied theme styles are scoped to the story content container.
- Runtime test: app shell, toolbar, account/auth UI, and global controls do not inherit Qwest theme typography or surface styles.
- Runtime test: user preferences for larger text, reduced motion, high contrast, and disabled ambience/effects override or neutralize theme values.
- Reader mode test: valid themes work in continuous mode.
- Reader mode test: valid themes work in paginated mode.

---

## Runtime Services & Resolver Acceptance Criteria & Tests

Runtime Services and Resolvers are successful when:

- Story Runtime, Interaction Runtime, Atmospheric Runtime, Media Runtime, Persistence Runtime, and Analytics Runtime can follow the same service organization model.
- Runtime subsystems are organized around services, managers, registries, modules, and resolvers.
- Atmospheric Runtime remains one runtime service among several, not a special architectural case.
- Runtime code resolves content targets, registry entries, flow targets, interactions, ambience, media, and nodes through typed resolvers.
- Resolvers return explicit success, fallback, or error results.
- Resolver behavior is deterministic for a given bundle, state, and registry set.
- Runtime code avoids ad hoc array searches for ID-based lookup.

Required tests:

- Unit test: a resolver returns a successful result for a valid ID.
- Unit test: a resolver returns an explicit fallback for a valid fallback case.
- Unit test: a resolver returns an explicit error for an unresolved ID.
- Unit test: resolver output is deterministic for the same bundle, state, and registry set.
- Unit test: Atmospheric Runtime managers are organized under the Atmospheric Runtime service.

---

## Platform Registry Acceptance Criteria & Tests

Platform Registry behavior is successful when:

- Themes, fonts, ambient effects, ambient transitions, ambient channels, interaction indicators, script containers, commands, and transition types use the same registry lifecycle.
- Qwest content references approved extension points by ID.
- Validation rejects or safely falls back for unknown registry IDs.
- Runtime resolves registry references through internal platform registries.
- Registry entries define capability metadata where runtime behavior depends on capabilities.
- Registry entries define requirements metadata where runtime compatibility depends on reader preferences, device capability, user activation, or accessibility mode.
- Deprecated registry entries can provide documented fallback behavior.
- Qwest content never stores executable implementation code for a registered extension point.
- The Platform Registry remains closed but extensible: creators extend through content, developers extend through approved registries, and users never execute arbitrary code.

Required tests:

- Unit test: a valid registry ID resolves to the expected registry entry.
- Unit test: an unknown registry ID fails validation or resolves to the documented safe default.
- Unit test: deprecated registry entries resolve through documented fallback behavior.
- Unit test: registry capability metadata is available to runtime managers.
- Unit test: registry requirements metadata lets runtime managers disable or substitute unsupported channels.
- Validation test: arbitrary third-party plugin declarations are rejected.
- Validation test: Qwest JSON with executable code in a registry-backed extension point is rejected.

---

## Ambient Reader Effects Acceptance Criteria & Tests

Ambient Reader Effects are successful when:

- Ambient modules are first-class, reusable reader enhancements.
- Ambient modules are resolved from an approved registry.
- Qwest JSON references ambient effect IDs without storing executable code.
- Atmospheric Runtime owns ambience orchestration through Theme Manager, Ambient Manager, Transition Manager, and Channel Manager.
- Ambient modules declare channel capabilities but do not own cross-effect transition orchestration.
- Keyword-driven ambience can trigger when matching prose becomes visible.
- Explicit ambience tags can trigger deterministic ambience.
- Ambient zones provide inherited ambience for authored story regions.
- Ambient transitions blend supported channels between active effects.
- Authors can disable ambience per node.
- Authoring preview and validation show detected ambience triggers.
- Authoring assist can show detected keyword ambience and allow accept, ignore, and always-ignore decisions.
- Ambience does not affect choices, unlocks, flags, endings, discoveries, achievements, score, progress, or reader-state paths unless a separate explicit Effect declares that behavior.
- Continuous reader mode triggers ambience only for visible content.
- Paginated reader mode triggers ambience only for content on the current page.
- `AmbientLayer` renders outside story text and does not affect text layout, pagination, or measurement.
- Ambient visuals use `pointer-events: none`.
- Reader global ambience disable turns off ambience.
- Ambient visual, audio, lighting, and haptic channels are independently controllable.
- Ambient audio does not start until the reader explicitly enables ambience audio.
- Ambient audio fades in and out smoothly.
- Ambient audio is never required for story comprehension or progression.
- Reduced-motion users receive static or disabled fallbacks.
- Ambience rejects arbitrary author-supplied JavaScript, CSS, CodePen embeds, external executable embeds, and unregistered effect IDs.

Required tests:

- Unit test: keyword matching uses word boundaries.
- Unit test: per-node `ambientAssist.enabled: false` disables ambience for that node.
- Unit test: reader global disable turns off ambience.
- Unit test: visual, audio, lighting, and haptic ambience preferences are independently applied.
- Unit test: invalid ambient effect IDs are rejected or ignored safely.
- Unit test: invalid ambient zone references are rejected or ignored safely.
- Unit test: ambient transitions resolve registered source and target effects.
- Unit test: Transition Manager owns cross-fade timing between active effects.
- Unit test: Channel Manager disables unsupported or user-disabled channels before modules render.
- Unit test: Ambient Manager resolves active zones, tags, rules, and cooldowns without changing story state.
- Integration test: a fog keyword activates the fog ambient module.
- Integration test: explicitly tagged ambience activates the selected module.
- Integration test: an ambient zone persists across multiple nodes until another zone starts or the zone ends.
- Integration test: fog transitions into rain using cross-fade timing instead of instant start/stop behavior.
- Integration test: paginated mode only triggers ambience for the visible page.
- Integration test: continuous mode triggers ambience when the matching block becomes visible.
- Integration test: ambient audio does not start until reader audio ambience is enabled.
- Runtime test: `AmbientLayer` does not change story text layout, pagination, or measurement.
- Runtime test: reduced-motion fallback is static or disabled.

## Qwest Metadata & Artwork Admin UI Acceptance Criteria & Tests

The canonical artwork and image-asset contract is defined by the
[Qwest Media Model](media.md#artwork). This section owns its cross-surface
acceptance evidence.

Qwest Metadata & Artwork Admin UI is successful when:

- Authors can edit Qwest title and artwork from a dedicated settings surface.
- Authors can assign cover, banner, thumbnail, and logo artwork from existing image media assets.
- Non-image media assets cannot be assigned as artwork.
- Missing artwork falls back to platform defaults.
- Artwork previews render for card, launch, and banner contexts.
- Published bundles include valid `qwest.artwork` references.
- Artwork changes do not alter runtime flow, reader progress, discoveries, achievements, scoring, or interactions.

Required tests:

- Unit test: valid artwork image IDs save into `QwestMetadata.artwork`.
- Validation test: missing artwork asset IDs are rejected or safely fallback.
- Validation test: non-image artwork assets are rejected.
- Runtime/export test: published bundle includes artwork metadata.
- UI test: artwork picker only allows image media assets.
- UI test: artwork preview renders cover, banner, thumbnail, and logo slots.
