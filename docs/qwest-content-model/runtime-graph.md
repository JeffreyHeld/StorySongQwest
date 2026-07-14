# Runtime Graph and Services

> **Documentation class: Canonical.** This document defines the durable runtime service, resolver, registry, atmosphere, theme, ambience, and safe extension boundaries. It was separated from the [Phase 4 implementation specification](../../story-song-qwest/docs/songqwest-storyqwest-phase-4-unified-content-model-runtime-platorm.md) so later phase status cannot silently redefine the contract.

[Content Model Overview](overview.md) · [Manuscript](manuscript.md) · [Sections](sections.md) · [Runtime Graph](runtime-graph.md) · [Media](media.md) · [Interactions](interactions.md) · [World](world.md) · [Maps](maps.md) · [Journal](journal.md) · [Progress](progress.md) · [Validation](validation.md)

## Runtime Services

Runtime Services are the shared organization model for runtime subsystems.

They keep runtime architecture consistent as StoryQwest grows.

Every major runtime subsystem should follow the same pattern:

```txt
Service
Managers
Registries
Modules
Resolvers
```

Runtime services may include:

- Story Runtime.
- Interaction Runtime.
- Atmospheric Runtime.
- Media Runtime.
- Persistence Runtime.
- Analytics Runtime.

Service contract:

```ts
type RuntimeService = {
  id: RuntimeServiceId;
  label: string;
  managers: RuntimeManager[];
  registries?: PlatformRegistry<PlatformRegistryEntry>[];
  modules?: RuntimeModule[];
  resolvers?: RuntimeResolver<unknown, unknown>[];
};

type RuntimeServiceId =
  | "story"
  | "interaction"
  | "atmospheric"
  | "media"
  | "persistence"
  | "analytics";

type RuntimeManager = {
  id: string;
  label: string;
  owns: string[];
};

type RuntimeModule = {
  id: string;
  registryKind: PlatformRegistryKind;
};
```

Rules:

- Services own runtime behavior for a subsystem.
- Managers own orchestration within a service.
- Registries own approved extension metadata.
- Modules provide approved implementation capabilities.
- Resolvers provide stable lookup and resolution APIs.
- Qwest content remains declarative and does not instantiate services, managers, or modules directly.

Atmospheric Runtime is one runtime service among several. It should not be a special architectural case.

---

## Resolver Pattern

Resolvers are the standard way runtime code answers ID-based questions.

Runtime code should ask a resolver to resolve an ID instead of searching arrays ad hoc.

Conceptual examples:

- Content Target Resolver.
- Registry Resolver.
- Flow Resolver.
- Interaction Resolver.
- Ambient Resolver.
- Media Resolver.
- Node Resolver.

Resolver contract:

```ts
type RuntimeResolver<TRef, TResult> = {
  kind: ResolverKind;
  resolve: (ref: TRef, context: ResolverContext) => ResolveResult<TResult>;
};

type ResolverKind =
  | "contentTarget"
  | "registry"
  | "flow"
  | "interaction"
  | "ambient"
  | "media"
  | "node";

type ResolverContext = {
  bundle: QwestBundle;
  readerState?: ReaderState;
  userState?: UserState;
  registries?: PlatformRegistry<PlatformRegistryEntry>[];
};

type ResolveResult<TResult> = {
  ok: boolean;
  value?: TResult;
  fallback?: TResult;
  error?: ResolveError;
};

type ResolveError = {
  code: string;
  message: string;
  ref?: unknown;
};
```

Rules:

- Runtime systems should resolve IDs through typed resolvers.
- Resolvers should return explicit success, fallback, or error results.
- Validation should use the same resolver concepts as runtime where practical.
- Resolver behavior must be deterministic for a given bundle, state, and registry set.
- Resolvers should hide storage shape so runtime code does not depend on scanning arrays directly.

The runtime question should be:

```txt
Resolve this ID.
```

not:

```txt
Search these arrays until something matches.
```

---

## Platform Registry Pattern

StoryQwest extension points should follow one approved registry pattern.

The pattern is:

```txt
Register
Validate
Reference by ID
Resolve at runtime
```

This keeps Qwest content declarative while allowing the platform to grow through approved capabilities.

Approved registries may include:

- Reader themes.
- Fonts.
- Ambient effects.
- Ambient transitions.
- Ambient channels.
- Interaction indicators.
- Interaction presentation renderers.
- Script container / mini-experience modules.
- Platform commands.
- Transition types.

Registry contract:

```ts
type PlatformRegistryEntry = {
  id: string;
  label: string;
  kind: PlatformRegistryKind;
  version?: string;
  deprecated?: boolean;
  capabilities?: string[];
  requirements?: PlatformRegistryRequirements;
  fallbackId?: string;
};

type PlatformRegistryKind =
  | "readerTheme"
  | "font"
  | "ambientEffect"
  | "ambientTransition"
  | "ambientChannel"
  | "interactionIndicator"
  | "interactionPresentation"
  | "scriptContainer"
  | "command"
  | "transition";

type PlatformRegistry<TEntry extends PlatformRegistryEntry> = {
  kind: PlatformRegistryKind;
  entries: Record<string, TEntry>;
  defaultId?: string;
};

type PlatformRegistryRequirements = {
  audio?: boolean;
  visual?: boolean;
  lighting?: boolean;
  haptic?: boolean;
  reducedMotionCompatible?: boolean;
  highContrastCompatible?: boolean;
  userActivationRequired?: boolean;
  deviceCapabilities?: string[];
};
```

Rules:

- Authored Qwest JSON references registry entries by ID.
- Authored Qwest JSON must not contain executable implementation code.
- Validation checks every registry reference before publish.
- Unknown IDs fail validation or fall back to a documented safe default.
- Runtime resolves IDs through internal registries.
- Registries own implementation details, capability flags, requirements, deprecation metadata, fallback behavior, and security boundaries.
- Content targets may point at approved commands or content objects, but commands still resolve through the platform command registry.
- Adding a new extension point should reuse this lifecycle instead of inventing a separate validation and resolution model.
- Registry requirements allow runtime services to make compatibility decisions without scattering device, preference, or accessibility logic throughout module code.

Example:

```json
{
  "id": "storm",
  "requirements": {
    "audio": true,
    "reducedMotionCompatible": true,
    "lighting": false
  }
}
```

If a reader disables audio, a runtime service may resolve a visual-only storm fallback. If a device does not support haptics, the Channel Manager can ignore the haptic channel automatically.

### Closed Extension Boundary

The Platform Registry is closed but extensible.

It is not a generic third-party plugin system.

Boundary:

- Creators extend experiences through declarative content.
- Developers extend platform capabilities through approved registries.
- Users never execute arbitrary author-supplied or third-party code.

This keeps the platform consistent with the Script Container security model: content is declarative and validated, while executable behavior lives in approved platform modules.

This pattern applies equally to the theme registry, ambient registry, font registry, interaction registry, command registry, transition registry, and approved mini-experience registry.

---

## Atmospheric Runtime

Atmospheric Runtime is the controlled reader atmosphere layer.

It has two complementary parts:

- Reader Theme — static atmosphere for typography and story surface presentation.
- Ambient Effects — dynamic atmosphere for visual, audio, lighting, haptic, and future reader-safe effect channels.

Atmospheric Runtime is not a design system, app skin, scripting environment, or story-state system.

It lets the platform enrich the reading experience while preserving the core Phase 4 boundary: Qwest content remains declarative, validated, and runtime-safe.

### Runtime Service Architecture

Atmospheric Runtime should be a runtime service, not only a collection of effects.

Conceptual services:

```txt
Atmospheric Runtime
  Theme Manager
  Ambient Manager
  Transition Manager
  Channel Manager
```

Responsibilities:

- Theme Manager resolves the effective reader theme from Qwest content, registry defaults, and user preferences.
- Ambient Manager resolves active ambient rules, tags, zones, cooldowns, and current reading position.
- Transition Manager owns timing, cross-fades, visual blends, channel transitions, and fallbacks between active effects.
- Channel Manager decides which visual, audio, lighting, haptic, and future channels are enabled for the current reader, device, preferences, and accessibility mode.

Ambient modules should not own orchestration.

Ambient modules know how to render or provide a channel.

Atmospheric Runtime knows when and how to blend modules.

Example runtime orchestration:

```txt
Fog starts

Visual:
fade 2.5 seconds

Audio:
fade wind in

Lighting:
coolBlue -> stormGray

Haptics:
off
```

This separation keeps modules small, reusable, and testable. A fog module renders fog. The runtime decides when fog starts, which channels are enabled, how it blends with rain, and how user preferences change the result.

### Reader Theme / Story Theme

Reader Theme is a controlled reader presentation layer for story atmosphere.

It is not a full design system.

It is not an app skinning system.

It lets a Qwest influence typography and story presentation inside the narrative reading surface without restyling the platform UI.

#### Scope

Reader Theme may apply only to the story content container rendered by the reader runtime.

Reader Theme may control:

- Story body font.
- Story heading font.
- Paragraph spacing.
- Line height.
- Reading column width.
- Quote, journal, and letter styling.
- Optional story surface background such as paper, parchment, dark paper, or minimal.
- Limited accent color for story-specific elements.

Reader Theme must not control:

- App navigation.
- Reader toolbar.
- Account or auth UI.
- Editor or creator UI.
- Global buttons and menus.
- Discovery, reveal, choice, or progression logic.
- Arbitrary CSS.
- Scripts.

The platform UI remains consistent across Qwests. A Qwest can only influence the narrative reading surface.

#### Theme Model

```ts
type ReaderTheme = {
  id?: ReaderThemeId;
  typography?: ReaderThemeTypography;
  surface?: ReaderThemeSurface;
  blocks?: ReaderThemeBlocks;
};

type ReaderThemeId =
  | "default"
  | "storybook-parchment"
  | "literary-paper"
  | "dark-paper"
  | "minimal";

type ReaderThemeTypography = {
  bodyFont?: ReaderFontToken;
  headingFont?: ReaderFontToken;
  scale?: "compact" | "comfortable" | "spacious";
  paragraphSpacing?: "tight" | "normal" | "relaxed";
  lineHeight?: number;
  maxWidth?: number;
};

type ReaderFontToken =
  | "system"
  | "serif-literary"
  | "sans-readable"
  | "mono-journal"
  | "fantasy-display";

type ReaderThemeSurface = {
  style?: "minimal" | "paper" | "parchment" | "darkPaper";
  accent?: ReaderAccentToken;
};

type ReaderAccentToken = "default" | "ember" | "moss" | "ink" | "gold" | "moon";

type ReaderThemeBlocks = {
  quote?: "plain" | "literary" | "ornate";
  journal?: "plain" | "lined" | "notebook";
  letter?: "plain" | "paper" | "sealed";
};
```

The concrete token lists may be implemented as an internal theme/font registry. The content contract must remain token-based. Qwest content must never provide raw CSS, style tags, script tags, external font URLs, or arbitrary class names.

Example:

```json
{
  "readerTheme": {
    "id": "storybook-parchment",
    "typography": {
      "bodyFont": "serif-literary",
      "headingFont": "fantasy-display",
      "scale": "comfortable",
      "lineHeight": 1.7,
      "maxWidth": 720
    },
    "surface": {
      "style": "parchment",
      "accent": "ember"
    }
  }
}
```

#### Validation Rules

Validation should treat Reader Theme as content metadata with strict boundaries.

Rules:

- Fonts must come from an approved internal theme/font registry or predefined theme token list.
- Theme IDs must come from the approved reader theme registry.
- Unknown theme IDs should fall back to the default reader theme.
- Invalid font, surface, block, spacing, scale, or accent tokens should be rejected by content validation or normalized to safe defaults.
- `lineHeight` must remain within an approved readable range.
- `maxWidth` must remain within an approved readable range.
- Raw CSS injection is not allowed.
- Script injection is not allowed.
- External font URLs are not allowed in Qwest content.
- Theme values must not create or alter runtime logic for reveals, discoveries, interactions, choices, scoring, or progression.

Validation errors should point to the Qwest-level `readerTheme` field whenever possible.

#### Runtime Behavior

Reader runtime resolves the effective theme from the published Qwest bundle.

Resolution order:

1. Start with the default reader theme.
2. Apply a valid registered theme by `readerTheme.id`.
3. Apply valid token overrides from `readerTheme.typography`, `readerTheme.surface`, and `readerTheme.blocks`.
4. Apply user reader preferences and accessibility settings last.

Runtime requirements:

- Reader applies theme values only to the story content container.
- App shell typography and platform controls must not inherit Qwest theme fonts or surface styles.
- Reader toolbar, account/auth UI, editor/creator UI, global buttons, and menus keep platform styling.
- User preferences for larger text, reduced motion, high contrast, and disabled ambience/effects override or neutralize authored theme values where needed.
- Theme resolution must work in continuous and paginated reader modes.
- Missing theme data uses the default reader theme.
- Unknown theme IDs use the default reader theme.
- Invalid values are rejected before publish or normalized to safe defaults at runtime.

Reader Theme may make a story feel like parchment, paper, dark paper, or a minimal reading surface. It must not change what the reader can discover, reveal, unlock, or complete.

---

### Ambient Reader Effects

Ambient Reader Effects are first-class, content-driven reader enhancements.

They provide story atmosphere through approved visual background effects and optional audio when the reader reaches designated text or when keyword rules match the prose.

Ambience is not a story-state mechanic by default.

Ambient effects must not affect choices, unlocks, flags, endings, score, achievements, discoveries, progress, or reader-state paths unless a separate explicit Effect declares that behavior.

Ambient effects must work in continuous reader mode and paginated reader mode.

#### Module System

Ambient effects should live in a dedicated implementation module system:

```txt
src/lib/ambient/
src/lib/ambient/types.ts
src/lib/ambient/registry.ts
src/lib/ambient/keywordRules.ts
src/lib/ambient/AmbientLayer.tsx
src/lib/ambient/effects/fog/
src/lib/ambient/effects/fireflies/
src/lib/ambient/effects/rain/
```

The content model references approved module IDs. It does not store executable effect code.

Shared module contract:

```ts
type AmbientEffectModule = {
  id: string;
  label: string;
  keywords: string[];
  channels: AmbientChannels;
  capabilities?: AmbientModuleCapabilities;
  defaultIntensity: AmbientIntensity;
  supportsReducedMotion: boolean;
};

type AmbientChannels = {
  visual?: React.ComponentType<AmbientEffectProps>;
  audio?: AmbientAudioConfig;
  lighting?: AmbientLightingConfig;
  haptic?: AmbientHapticConfig;
};

type AmbientEffectProps = {
  intensity: AmbientIntensity;
  reducedMotion: boolean;
  highContrast: boolean;
};

type AmbientAudioConfig = {
  assetId: string;
  type?: "loop" | "oneShot";
  loop: boolean;
  defaultVolume: number;
  fadeInMs: number;
  fadeOutMs: number;
};

type AmbientLightingConfig = {
  tint?: AmbientColorToken;
  intensity?: AmbientIntensity;
};

type AmbientHapticConfig = {
  pattern: "none" | "lightPulse" | "singleTap" | "rumble";
  requiresUserActivation: boolean;
};

type AmbientModuleCapabilities = {
  visualBlends?: ("fade" | "dissolve")[];
  supportsAudioFade?: boolean;
  supportsLightingBlend?: boolean;
  supportsHaptics?: boolean;
};

type AmbientColorToken =
  | "coolBlue"
  | "warmGold"
  | "stormGray"
  | "moonlight"
  | "ember";

type AmbientIntensity = "subtle" | "medium" | "strong";
```

The registry owns React components, audio channel definitions, lighting channel definitions, haptic channel definitions, performance constraints, and reduced-motion fallbacks. Qwest JSON owns only declarative references and rule configuration.

Ambient modules declare capabilities. They do not orchestrate transitions.

Transition Manager decides how to apply those capabilities when the active ambience changes.

The runtime asks each resolved module which channels are enabled for the current reader:

```txt
Render Visual?
Render Audio?
Render Lighting?
Render Haptics?
```

Reader settings, device capability, permissions, high-contrast mode, reduced-motion mode, and platform policy can independently disable or simplify any channel.

#### Qwest JSON Contract

Qwest bundles may declare ambient effect registry references, keyword mappings, explicit tags, ambient zones, global ambient assist defaults, and per-node overrides.

```ts
type AmbientEffectRef = {
  effectId: string;
  intensity?: AmbientIntensity;
};

type AmbientRule = {
  id: string;
  keywords: string[];
  effectId: string;
  trigger: AmbientTrigger;
  cooldown?: number;
  intensity?: AmbientIntensity;
  transition?: AmbientTransition;
};

type AmbientTrigger = "onReach" | "onVisible" | "explicit";

type AmbientAssistDefaults = {
  enabled: boolean;
  keywordRulesEnabled?: boolean;
  explicitTagsEnabled?: boolean;
  visualsEnabled?: boolean;
  audioEnabled?: boolean;
  defaultIntensity?: AmbientIntensity;
};

type AmbientAssistOverride = {
  enabled?: boolean;
  ambientZone?: string;
  effectIds?: string[];
  ruleIds?: string[];
  explicitEffects?: AmbientEffectRef[];
};

type AmbientTag = {
  id: string;
  effectId: string;
  sourceNodeId: string;
  sourceRunId?: string;
  trigger: AmbientTrigger;
  intensity?: AmbientIntensity;
  transition?: AmbientTransition;
};

type AmbientZone = {
  id: string;
  label?: string;
  effectIds: string[];
  startsAtNodeId?: string;
  endsAtNodeId?: string;
  transition?: AmbientTransition;
  channels?: AmbientChannelPreference;
};

type AmbientTransition = {
  from?: string;
  to: string;
  crossFadeMs?: number;
  visualBlend?: "fade" | "dissolve";
};

type AmbientChannelPreference = {
  visual?: boolean;
  audio?: boolean;
  lighting?: boolean;
  haptic?: boolean;
};
```

Example keyword rule:

```json
{
  "ambientRules": [
    {
      "id": "fog-rule",
      "keywords": ["fog", "mist", "haze"],
      "effectId": "fog",
      "trigger": "onReach",
      "cooldown": 30
    }
  ]
}
```

Per-node override example:

```json
{
  "ambientAssist": {
    "enabled": false
  }
}
```

Per-node `ambientAssist` disables or constrains ambience for that node only. It must not alter reader progress, reveal behavior, discovery state, choices, flags, unlocks, or endings.

`ambientTags` support deterministic ambience for designated text or blocks. They may reference a node and, when compiled from rich text, the stable text run that should activate the effect.

Ambient zones support authored regions where ambience persists until another zone begins or the zone ends.

Example ambient zone:

```json
{
  "ambientZones": [
    {
      "id": "forest-night",
      "label": "Forest Night",
      "effectIds": ["fog", "wind", "owls"],
      "startsAtNodeId": "chapter-3",
      "endsAtNodeId": "chapter-4",
      "transition": {
        "to": "forest-night",
        "crossFadeMs": 2500,
        "visualBlend": "dissolve"
      }
    }
  ]
}
```

Per-node zone override example:

```json
{
  "ambientAssist": {
    "ambientZone": "forest-night"
  }
}
```

Keyword triggers are the automatic mode.

Ambient zones are the authored director mode.

Both modes must coexist. A zone provides the inherited baseline ambience for a region, while keyword rules and explicit tags may add, intensify, or transition effects for specific prose moments.

#### Ambient Transitions

Ambient transitions let long Qwests change atmosphere naturally instead of stopping one effect and instantly starting another.

Example:

```txt
Fog
Rain
Thunderstorm
Clear night
```

When a new ambient rule, tag, or zone resolves to a different effect, the runtime should use `AmbientTransition` to blend channels where supported.

Example transition:

```json
{
  "transition": {
    "from": "fog",
    "to": "rain",
    "crossFadeMs": 2500,
    "visualBlend": "fade"
  }
}
```

Runtime behavior:

- Fade outgoing audio while fading incoming audio where audio channels are enabled.
- Fade or dissolve visual layers where visual channels are enabled.
- Blend lighting tokens where lighting channels are enabled.
- Trigger haptics only when the reader has enabled haptics and the device supports them.
- Own cross-channel timing so individual ambient modules do not need to implement their own transition orchestration.
- Fall back to a safe immediate change if a module does not support a requested transition.
- Respect reduced-motion preferences by shortening, simplifying, or disabling visual transitions.

#### Authoring Behavior

Authors can rely on keyword-driven ambience.

Example prose:

```txt
As they entered the gates a fog rolled in...
```

If a Qwest has an ambient rule for `fog`, the reader may trigger the fog ambience when the prose block becomes visible or is reached.

Another example:

```txt
The wind picked up as snow began falling through the dead trees.
```

Authoring assist may detect `snow` and `wind` and show a detected ambience badge:

```txt
Detected ambience

Snow
Wind

[Accept]
[Ignore]
[Always Ignore Snow]
```

Authors can:

- Use keyword-driven ambience.
- Define ambient zones for authored regions.
- Disable ambience per node.
- Explicitly tag ambience when deterministic control is needed.
- Preview detected ambience triggers.
- Approve, remove, or adjust detected triggers before publish.
- Always ignore a detected keyword or effect when it is not appropriate for the story.

Preview and validation should show detected ambience triggers against the relevant text block so the author can understand why an effect will appear.

#### Runtime Behavior

The reader resolves ambient rules from the published Qwest bundle and the approved ambient module registry.

Runtime requirements:

- Use `IntersectionObserver` or equivalent visibility tracking to trigger ambience when the relevant block or text becomes visible.
- In continuous mode, trigger ambience only for content that enters the reader viewport.
- In paginated mode, trigger ambience only for content on the current page.
- Render `AmbientLayer` outside the story text so ambience does not affect text layout, pagination, line measurement, or story-surface sizing.
- Ambient visuals must use `pointer-events: none`.
- Effects must be safe for mobile performance.
- Reduced-motion users receive static or disabled fallbacks.
- High-contrast mode may neutralize or simplify ambience when needed.
- Global reader ambience disable turns off all ambience channels.
- Visual, audio, lighting, and haptic ambience channels are independently controllable.
- Ambient zones provide inherited baseline ambience until another zone starts, the zone ends, or the Qwest ends.
- Ambient transitions blend supported channels between active effects instead of requiring instantaneous start/stop changes.

Ambient effects may respond to reading position, visibility, cooldowns, and reader preferences. They must not create hidden runtime flow.

#### Audio Rules

Ambient modules may include optional audio loops or one-shot sound cues.
Canonical asset identity, ambient-audio relationships, playback boundaries,
and accessibility requirements are defined by the [Qwest Media Model](media.md#ambient-audio).
Atmospheric Runtime owns the orchestration of those rules across active effect
modules and enabled channels; it does not redefine the media contract.

#### Security Boundaries

Ambience must preserve the platform rule that content is declarative and validated.

Rules:

- Do not allow arbitrary author-supplied JavaScript.
- Do not allow arbitrary author-supplied CSS.
- Do not allow CodePen or external executable embeds inside Qwest content.
- Ambience must use approved registry modules only.
- Qwest JSON references effect IDs.
- Qwest JSON does not store executable effect code.
- Unknown effect IDs should fail validation or fall back to no ambience.
- Keyword matching must use safe text processing with word boundaries.

---

---

## Script Container Nodes

Script Containers allow embedded mini-experiences.

Examples:

- Quiz
- Trivia
- Puzzle
- Matching game
- Memory game
- Word search

```ts
type ScriptContainerNode = {
  id: string;
  scriptType: string;
  config: Record<string, unknown>;
};
```

Supported launch types:

```ts
quiz;
trivia;
memory;
matching;
wordsearch;
puzzle;
```

---

## Script Security Model

Phase 4 does not support arbitrary JavaScript.

Creators configure approved runtime modules.

```ts
approvedRuntime;
sandboxedRuntime;
validatedConfig;
```

No user-supplied executable code.

No unrestricted script execution.
