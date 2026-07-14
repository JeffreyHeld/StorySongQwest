# Structural Sections

> **Documentation class: Canonical.** This document defines the durable structural Section authoring model and its semantic and placement metadata shared by StoryQwest and SongQwest. It was separated from the [Phase 4 implementation specification](../../story-song-qwest/docs/songqwest-storyqwest-phase-4-unified-content-model-runtime-platorm.md) so later phase status cannot silently redefine the contract.

[Content Model Overview](overview.md) · [Manuscript](manuscript.md) · [Sections](sections.md) · [Runtime Graph](runtime-graph.md) · [Media](media.md) · [Interactions](interactions.md) · [World](world.md) · [Maps](maps.md) · [Journal](journal.md) · [Progress](progress.md) · [Validation](validation.md)

## Manuscript Structure & Node Authoring

Phase 4Q defines how an author's manuscript naturally maps to a structural runtime graph while preserving the absolute architectural separation between manuscript structure and runtime flow.

The outline, Journey Map, validation layers, runtime graph, and exported bundle become unified, deterministic views of the exact same underlying content structure.

Outline ──> Journey Map ──> Validation ──> Runtime Graph ──> Bundle

Runtime graph generation is deterministic. Given the same manuscript, compiler version, and settings, the resulting runtime graph must be identical. This ensures preview, validation, publishing, Journey Maps, analytics, and replay all operate on the same canonical structure.

### Structural Node Palette & Authoring Concepts

Authors interact with narrative concepts inside the workspace, which map cleanly to established canonical Phase 4 runtime node types:

- **General Section (`nodeType: "section"`)**: The default narrative block
  container. `sectionType` and `sectionRole` are omitted, so it uses general
  narrative semantics in the main body. Headings and paragraphs remain rich
  text formatting within this section.
- **Scene, Song, Dialogue, Journal, or Letter (`nodeType: "section"`)**: Authored
  semantic varieties expressed through `sectionType`. They retain the same
  runtime graph behavior as a General Section.
- **Prologue, Epilogue, or Credits (`nodeType: "section"`)**: Structural
  placement expressed through `sectionRole`. A role may be combined with any
  valid `sectionType` and does not create a new runtime node behavior.
- **Choice Hub (`nodeType: "choiceHub"`)**: Interrupts automatic continuation and presents choices as reader actions.
- **Branch (`nodeType: "branch"`)**: A choice target with its own content, title, document order, validation, previewability, and continuation flow.
- **Ending (`nodeType: "ending"`)**: A terminal destination that participates in validation, replays, achievements, and Journey Maps.
- **Hidden Section**: An authoring surface concept that compiles directly to a canonical **`nodeType: "hiddenNode"`**. This keeps the authoring paradigm natural while maintaining a pristine, unbloated runtime model.

Every **structural Manuscript Section** compiles into one canonical `QwestNode`.
`SectionType` describes what the section contains; `SectionRole` describes where
it sits in the work. When `sectionType` is omitted, the section uses general
narrative semantics. When `sectionRole` is omitted, it belongs to the main body.
Future roles such as Epigraph or Title Page require an explicit vocabulary
addition rather than an implicit custom runtime node.

Choices remain metadata edges/actions from a `choiceHub` to target branch nodes. They are not standalone runtime nodes in Phase 4Q.

#### Heading Behavior vs. Node Boundaries

Headings within a structural Manuscript Section are treated as formatting metadata, not node creators. The editor provides visual macro actions to let authors adjust node boundaries safely:

- **New Section** / **New Scene**
- **Split at Heading**
- **Convert Heading to Section**

---

### Compiler-Generated Automatic Flow

Authors do not manually connect simple linear progression paths. Manuscript files own sections and sequential order, while the compiler explicitly owns the generation of the `flow[]` array.

- **Rule**: During compilation, the compiler resolves implicit linear continuation into explicit runtime flow based on `documentOrder`.
- **Example**:
  `Section 1` ──> `Section 2` ──> `Section 3`
  automatically compiles to:
  - **Node A**: `flow -> Node B`
  - **Node B**: `flow -> Node C`
  - **Node C**: Evaluated as `terminal` unless choices, explicit flow definitions, or an interactive override redirect it.

---

### Migration Strategy & Compatibility

Phase 4Q introduces a non-breaking authoring workflow advancement rather than invalidating deployed or legacy content.

#### Backward Compatibility Guarantees

- **Zero Runtime Breakage**: Existing Qwests remain valid and continue to render through the legacy-compatible compilation/runtime path until explicitly migrated.
- **No Forced Migration**: Existing manuscripts authored prior to Phase 4Q continue to load as-is. The compiler continues to support the legacy single-section compilation path alongside the new multi-node structural system.
- **Author-Driven Adoption**: Creators choose exactly when to migrate a legacy manuscript to the structured model.

#### Creator Studio Migration Utilities

The editor surface provides idempotent, author-controlled tools to transition legacy documents over to the new structural model:

- **Split Manuscript into Sections**
- **Convert Headings to Sections**
- **Preview Section Boundaries**

Existing manuscripts containing legacy choices continue to compile unchanged.

Once a creator triggers and executes migration, the manuscript adopts the canonical Phase 4Q structured section ruleset for all future validation and bundle exports.

---

### Revised Manuscript Validation Rules

The platform validation engine enforces the structural integrity of the manuscript graph prior to compilation and deployment:

- **1:1 Export Integrity**: Every unique structural Manuscript Section, Ending, and Hidden Section must compile into exactly one discrete runtime node with a unique, unalterable ID. Duplicate IDs fail validation.
- **Automatic Continuation Constraints**: Linear sections must successfully resolve their implicit downstream node targets via `documentOrder`.
- **Terminal Integrity**: Nodes designated as an Ending type must not contain outgoing flow properties. Any outgoing paths on an ending node will trigger a compilation failure.
- **Orphan and Ghost Detection**: Empty manuscript sections generate validation warnings. Hidden sections (`hiddenNode`) lacking a correlating conditional flag or structural reveal path generate an "unreachable node" validation warning.
- **Boundary Alignment**: Compilation will abort if a runtime-compiled interaction offset range slips or breaks configuration alignment with its parent text run boundaries.

---

### Manuscript & Node Authoring Acceptance Criteria & Tests

#### Success Criteria

- A migrated or newly created manuscript containing five discrete structural sections exports exactly five distinct runtime nodes.
- Hidden Sections compile into runtime nodes flagged initialized as `nodeType: "hiddenNode"` with valid criteria.
- Endings compile cleanly as terminal nodes with zero outgoing paths.
- Branches are first-class runtime nodes and are addressable, validatable, previewable, and targetable.
- Authored Scene, Song, Dialogue, Journal, and Letter semantics are preserved
  through `sectionType` without creating separate runtime node types.
- Prologue, Epilogue, and Credits placement is preserved through `sectionRole`
  without changing runtime flow.
- A completely linear manuscript successfully resolves flow tracking dynamically from start to finish without requiring any manual wiring from the author.
- Journey Maps render exactly one node symbol per authored manuscript section.
- Converting a text heading into a section accurately splits the text block contents while injecting a new runtime node boundary cleanly into the pipeline.
- Legacy Phase 4 bundles continue to parse safely using runtime default fallback mappings.

#### Required Tests

- **Unit Test**: Verify the bundle compiler automatically populates the `flow` target property of an upstream node with the ID of the immediate next node according to `documentOrder`.
- **Validation Test**: Assert that a validation error is thrown if a node of type ending is configured with an active outgoing `flow` route.
- **Compiler Test**: Confirm via deep assertion that a multi-section manuscript produces an equivalent length array inside `QwestBundle.nodes`, directly rejecting any attempt to collapse multiple authored section chunks into a single node.
- **Migration Test**: Run an import regression against an unsegmented legacy manuscript string to ensure the platform generates a valid baseline section node configuration gracefully without corrupting underlying string interaction arrays.
