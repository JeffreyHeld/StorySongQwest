# Qwest Presentation Metadata

`readerTheme` is optional authored Qwest-level presentation metadata. It is
consumed by Creator Studio, preview, publishing, and the StoryQwest Reader.
Presentation metadata does not alter node identity, section semantics, runtime
graph topology, interactions, progress, or narrative state.

## Separate presentation axes

- **Foundation environment** (`environment`) is a coordinated semantic palette
  identifier: `mysticalCosmic`, `cyberSynthesis`, `eldritchUnderdark`,
  `solarDawn`, or `overgrownRunes`. It describes the world-level palette used
  by the Reader.
- **Reader preset** (`preset`, with legacy `id` support) is an authoring
  convenience. It supplies coordinated Reader defaults for typography, spacing,
  reading surface, and authored content blocks. It is not an environment.
- **Reading surface** (`readingSurface`) is the visual reading-plane treatment
  rendered inside the runtime Experience Surface. It is not the Experience
  Surface itself. Existing `surface.style` input is accepted and normalized to
  `readingSurface`.
- **Accent** (`accent`, with legacy `surface.accent` support) is semantic
  emphasis resolved within the active environment. It is not an environment.
- **Appearance** (System, Light, or Dark) remains an audience/platform
  preference and is not authored in the published content model.

Reader presets and environments are independent, so combinations such as
Eldritch Underdark + Dark Paper and Solar Dawn + Dark Paper are valid.

## Compatibility and ownership

Publishing and runtime resolution use one normalized authored Reader theme
contract. Missing optional values use deterministic runtime defaults; a missing
environment defaults to `mysticalCosmic`, the established neutral compatibility
environment. Legacy `readerTheme.surface` remains readable, with `style`
normalized to `readingSurface` and `accent` normalized to `accent`. Runtime
resolution never mutates authored values.

The published bundle owns authored presentation metadata. The runtime owns
semantic token resolution, appearance adaptation, forced-colors behavior,
contrast safeguards, and platform accessibility preferences. Audience Reader
Preference Overrides are runtime/user preference state and are not part of the
published content model.
