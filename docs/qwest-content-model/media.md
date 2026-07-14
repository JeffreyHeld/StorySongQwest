# Qwest Media Model

> **Documentation class: Canonical.** This document defines Qwest media identity,
> metadata, authored relationships, publishing, playback intent, transport
> boundaries, and accessibility requirements. Phase specifications and runtime
> implementation records may add evidence, but they do not supersede this
> contract.

[Content Model Overview](overview.md) · [Manuscript](manuscript.md) · [Sections](sections.md) · [Runtime Graph](runtime-graph.md) · [Media](media.md) · [Interactions](interactions.md) · [World](world.md) · [Maps](maps.md) · [Journal](journal.md) · [Progress](progress.md) · [Validation](validation.md)

## Core Principle

Media is a first-class Qwest content object, not a filename, storage path, or
renderer instance.

Creators place media at meaningful narrative moments. The platform manages
reusable assets, metadata, publishing, validation, versioning, optimization,
and delivery behind that authoring experience. A physical file may carry a
media object, but file presence alone does not establish the object's meaning.

The canonical relationship is:

```text
authored media intent
  -> stable media ID and logical media object
  -> published media snapshot
  -> availability/resolution handoff
  -> runtime presentation or playback
```

## Ownership Boundary

- **Authoring** owns asset selection, narrative role, anchors, captions,
  transcripts, lyrics, timing intent, overrides, and fallback intent.
- **The canonical bundle** owns stable media IDs, logical media objects,
  authored relationships, published metadata, storage references, version and
  integrity evidence, and declared requirement classification when present.
- **The compiler** validates references, normalizes authoring aliases, resolves
  draft library records into immutable published snapshots, and removes
  admin-only fields.
- **The Experience Platform** owns availability classification, cache/download
  policy, offline evaluation, integrity policy, and the runtime-safe resolution
  handoff. It does not own playback.
- **StoryQwest and SongQwest runtimes** own rendering, media elements, controls,
  preload timing, playback, lyric/transcript presentation, fallback, progress,
  resume, mounted errors, and teardown.

No layer may replace a canonical media ID with a filename, URL, cache key, DOM
element, or playback object as the durable identity.

## Media Assets

The media library is the reusable authoring source. Published Qwest versions
contain immutable media snapshots sufficient to answer:

> What exactly did the audience see, hear, and play in this version?

```ts
type MediaType = "image" | "audio" | "video" | "other";

type PublishedMediaAsset = {
  id: string;
  type: MediaType;
  title: string;
  description?: string;
  altText?: string;
  caption?: string;
  transcript?: string;
  posterMediaId?: string;
  immutableAssetVersionId?: string;
  storagePath: string;
  contentType?: string;
  sizeBytes?: number;
  contentHash?: string;
};
```

Draft library records may additionally contain tags, usage references,
creator/updater identity, mutable timestamps, and soft-deletion metadata. Those
administrative fields are not part of the published runtime contract.

`storagePath` is a runtime storage reference, not necessarily a public URL or a
physical ZIP path. Imported bundles resolve it through manifest transport
mappings. Hosted runtimes resolve it through the injected media resolver.

Published media rules:

- IDs are stable within the Qwest version and are the only authored reference.
- `type` describes the logical media class, not merely the filename extension.
- `contentType` provides format evidence when known; it does not guarantee
  decoder support on every device.
- `contentHash` and `immutableAssetVersionId` provide integrity/version evidence
  when available; mutable URLs are not asset identity.
- `posterMediaId` must resolve to an image asset.
- Missing optional metadata must degrade safely.
- Missing required media must produce a deterministic validation or
  availability result rather than a guessed replacement.

### Media References

Content refers to media by ID and may add usage-specific presentation metadata:

```ts
type NodeMediaRef = {
  mediaId: string;
  role?:
    | "inline"
    | "background"
    | "reveal"
    | "poster"
    | "audio"
    | "video"
    | "other";
  captionOverride?: string;
  altTextOverride?: string;
  transition?: Transition;
};
```

Overrides apply only at that use site. They do not mutate the shared media
asset. Authoring concepts such as ambient, spotlight, hidden placeholder, or
reward must compile into canonical references, interactions, discoveries,
transitions, or node behavior rather than expanding `MediaType`.

## Image

Images use `type: "image"` and should provide intrinsic display metadata when
the publishing pipeline supports it. The current published contract guarantees
identity, storage reference, format evidence, and accessibility metadata; it
does not require a particular responsive-image or transformation provider.

Rules:

- Meaningful images require useful `altText` or a context-specific override.
- Decorative images must be explicitly treated as decorative by the runtime;
  an empty accessible name must not be inferred from a missing field.
- Images may be inline, background, revealed, discovered, used as posters, or
  assigned to Qwest artwork roles.
- Background and atmospheric images must not reduce text readability or bypass
  high-contrast preferences.
- Runtime sizing and optimization must preserve aspect ratio unless the
  authored presentation explicitly defines a safe crop.

## Audio

Audio uses `type: "audio"`. SongQwest and StoryQwest share the same media asset
contract; SongQwest adds audio-first profile references rather than introducing
a separate bundle or asset model.

```ts
type SongQwestAudioRole =
  | "primary"
  | "vocal"
  | "instrumental"
  | "ambience"
  | "transition"
  | "narration"
  | "fallback";

type SongQwestAudioRef = {
  mediaId: string;
  role: SongQwestAudioRole;
};
```

Audio rules:

- Authoring owns the role and fallback intent; runtime services own playback.
- Primary audio has priority over incidental media audio unless authored
  interruption behavior says otherwise.
- Audio may pause, duck, continue, replace, or mix with existing playback only
  when that intent is explicit and supported.
- Browser autoplay, device routing, buffering, Media Session integration, and
  interruption behavior are runtime concerns.
- Audio must have readable fallback content when it carries narrative meaning.

## Video

Video uses `type: "video"` and may reference an image through `posterMediaId`.

Rules:

- Video presentation may be inline, modal, overlay, fullscreen, revealed, or
  synchronized when supported by authored metadata.
- Video with audio must declare or inherit a deterministic interruption policy
  relative to primary audio.
- Closing modal or overlay video restores focus and the prior reader/listener
  location.
- Captions, transcript fallback, keyboard controls, and screen-reader labeling
  are required according to the accessibility rules below.
- External embeds, if ever supported, remain sandboxed transport surfaces and
  are not equivalent to canonical video assets.

## Waveforms

Waveforms are derived presentation or diagnostic data. They are not the source
audio, a narrative object, or a separate media type.

- A runtime or authoring tool may derive waveform samples from an audio asset.
- Derived samples must be versioned against the stable media ID and immutable
  asset evidence so stale data can be discarded.
- Waveform absence must never block playback, lyrics, captions, transcripts,
  progress, or publishing.
- Waveform rendering must have a non-visual playback-position alternative.
- Recording, trimming, waveform editing, stem editing, mixing, mastering, MIDI,
  and production-timeline editing are outside the canonical import-first Qwest
  workflow.

## Lyrics

Lyrics use canonical `TextRun` content. They do not create a parallel lyric-only
document or media-asset model.

```ts
type SongQwestLyricRole =
  | "lead"
  | "harmony"
  | "backing"
  | "spoken"
  | "annotation"
  | "interactionPrompt";

type TextRun = {
  id: string;
  text: string;
  songRole?: SongQwestLyricRole | string;
  songSegmentId?: string;
  songSegmentName?: string;
  startTimeMs?: number;
  endTimeMs?: number;
  interaction?: AuthoredTextInteraction;
};
```

Lyrics may associate with audio, Song segments, timing, inline interactions, and
fallback text. They remain readable, searchable, validatable, and available to
assistive technology without audio or specialized lyric rendering.

Lyrics are not a substitute for a complete transcript when spoken words,
non-lyrical narration, or meaningful non-speech audio also matter.

## Timed Lyrics

Timing is optional presentation metadata. It enhances playback but never
defines story structure or progression.

- Times are normalized in milliseconds relative to the owning audio asset.
- Start times must be non-negative and monotonically ordered within their
  sequence.
- An end time, when present, must be greater than or equal to its start time.
- Timing should fit known audio duration when trustworthy duration metadata is
  available.
- Partial or approximate timing is allowed when clearly treated as such.
- Missing timing degrades to static lyrics, a transcript, a note, or another
  readable presentation; it does not invalidate the runtime graph.
- Highlighting, scrolling, revealing, and interaction cues must respect reduced
  motion and must not create disruptive screen-reader live-region updates.

## Artwork

Qwest artwork is presentation metadata backed by image media assets:

```ts
type Artwork = {
  coverImageId?: string;
  bannerImageId?: string;
  thumbnailImageId?: string;
  logoImageId?: string;
};
```

- `coverImageId` is the primary cover image.
- `bannerImageId` is an optional wide hero image.
- `thumbnailImageId` is an optional compact card or search image.
- `logoImageId` is an optional title or logo treatment.

Artwork may appear in libraries, launch screens, continue cards, search,
collections, sharing previews, marketplace surfaces, and creator profiles.
Every reference must resolve to a valid image asset. Missing artwork falls back
to platform defaults. Artwork never changes flow, interactions, discoveries,
achievements, scoring, or progress.

## Ambient Audio

Ambient audio is an audio media asset used through an ambience reference or an
approved Atmospheric Runtime module. It is not hidden story state.

- Authored content declares environment or ambience intent and stable media
  references; the runtime chooses supported orchestration.
- Ambient audio starts only after explicit reader audio activation.
- It fades in and out safely and respects global audio, ambience, volume,
  reduced-motion, high-contrast, and device preferences where relevant.
- It is never required for comprehension or progression.
- Disabling ambient audio must not disable supported visual, lighting, or haptic
  ambience channels.
- Ambient audio does not compete with primary SongQwest audio without an
  explicit and supported mixing or ducking rule.

## Media Moments

A media moment is a narrative use of a media asset. It is not a second copy of
the asset.

Media may be anchored to a sentence, paragraph, structural section, choice,
reveal, lyric line, timestamp, ending, or addressable node. Depending on its
behavior, a media moment may compile as:

- a `NodeMediaRef` contained by narrative content;
- a `mediaMoment` node when it needs independent addressability and flow;
- an inline interaction or content target;
- a reveal or discovery relationship; or
- a runtime media event such as show, hide, or discover.

Presentation hints such as inline, modal, background, overlay, or fullscreen do
not create separate engine behavior. Media overlays preserve reader/listener
position, restore focus on close, and follow deterministic simultaneous-media
rules.

## Playback Metadata

Canonical content stores authored playback intent and stable resume anchors,
not a live player state machine.

Durable metadata may include:

- media ID and semantic role;
- content type and encoded format evidence;
- size, content hash, and immutable asset version;
- poster reference;
- lyric and Song-segment timing;
- resume checkpoint references; and
- caption, transcript, and fallback associations.

Current time, buffered ranges, volume, mute state, decoded buffers, active media
elements, retry counters, and device routes are transient runtime state. Only
meaningful resume data is projected into `QwestProgress`; the entire playback
state is never persisted as content.

The current published media asset contract does not standardize duration,
dimensions, codecs, bitrates, or text-track arrays. Consumers must not assume
those fields until a versioned contract adds them. Validators may use reliable
compile-time duration evidence when available without making it mandatory.

## Streaming

Streaming is a delivery capability, not a media type or authored identity.

- Bundles reference stable media IDs and storage references, never hard-coded
  provider behavior.
- Runtime-safe URLs or opaque handles arrive through the media resolver.
- Signed URL, CDN, bundled-file, data URL, and future streaming resolvers may
  implement the same boundary.
- An expired or replaced URL refreshes location only; it does not create a new
  logical media object.
- Capability negotiation distinguishes supported, unsupported, and
  decoder-dependent formats.
- Required media may block launch or offline readiness according to declared
  requirement classification; optional media degrades through runtime fallback.

## Caching

Caching, downloads, and offline availability are Experience Platform policy,
not authored Qwest content.

Cache identity includes registration, experience, bundle ID, bundle version,
canonical media ID, and integrity evidence. It excludes mutable URLs and
filenames. Different published versions must not collide because they reuse a
path or filename.

Rules:

- Active runtime assets remain protected until their use lease is released.
- URL expiry refreshes the location rather than invalidating content identity.
- Version, registration, or integrity changes invalidate according to policy.
- Partial downloads and integrity failures remain explicit availability states.
- Cache/download records never contain or mutate Qwest progress.
- Cached bytes do not prove that a logical media object or authored relationship
  exists; the bundle remains authoritative.

## Subtitles

Subtitles translate or transcribe spoken dialogue for time-based media. They
remain distinct from general descriptive captions and from SongQwest lyrics.

The current published contract supports transcript and caption content but does
not yet standardize a versioned subtitle-track array. Until that contract is
defined:

- runtimes may consume subtitle assets when explicitly present in the bundle;
- validators must not invent subtitle tracks from filenames;
- language, timing, format, and default-track behavior must not be assumed from
  storage paths; and
- absence of a specialized track must fall back to transcript or other readable
  content when the media carries narrative meaning.

## Captions

Captions communicate dialogue and meaningful non-speech audio. The canonical
asset-level `caption` is concise reader-facing content; `transcript` provides a
longer readable alternative.

- Video should expose captions when captions are present in the bundle.
- Audio should expose transcripts or captions when provided.
- Captions must identify meaningful sound when needed for comprehension.
- A use-site `captionOverride` may adapt shared media to local narrative context
  without mutating the asset.
- Captions and transcripts remain available independently of autoplay, timing,
  waveform, animation, or playback success.

## Accessibility

Media accessibility metadata is part of the content contract; accessible
rendering and controls are runtime responsibilities.

- Meaningful images provide alt text; decorative images are explicitly marked
  decorative by presentation context.
- Audio-first content provides lyrics, captions, transcript, notes, or other
  readable fallback appropriate to its meaning.
- Video provides captions and transcript fallback when required for
  comprehension.
- Media controls are keyboard and screen-reader operable and expose meaningful
  names, state, and errors.
- Modal media traps focus while open and restores focus to its trigger on close.
- Timed highlighting is never the only indicator of playback progress or
  current lyric.
- Reduced motion affects media transitions, overlays, lyric animation,
  visualizers, waveform motion, and animated artwork.
- High contrast preserves readable text, controls, captions, lyrics, and media
  overlays.
- Color, motion, audio, hover, and press-and-hold are never the only means of
  conveying required information or activating media.
- Unavailable media preserves canonical progress and presents deterministic
  fallback rather than trapping the audience.

## Validation

Publishing must reject or diagnose:

- missing media IDs or references;
- logical type and referenced-role mismatches;
- missing required primary audio;
- invalid or missing artwork image references;
- invalid poster references;
- broken lyric, Song-segment, media-event, interaction, or resume anchors;
- negative, reversed, or non-increasing timing;
- required accessibility metadata with no acceptable fallback;
- missing manifest mappings or packaged files when the transport declares them;
  and
- unsupported required formats or failed integrity evidence when known.

Warnings are appropriate when content remains playable but lacks recommended
metadata, such as optional timing. Errors are appropriate when a required asset
or relationship cannot resolve. Validation reports against the authored use
site whenever possible while preserving the stable media ID in diagnostics.

## Non-Goals

This model does not define:

- recording, waveform editing, audio-timeline editing, stem editing, mixing,
  mastering, MIDI, or live-production tools;
- a separate SongQwest media library, bundle, or progress system;
- a specific CDN, streaming provider, storage vendor, cache database, service
  worker, or download implementation;
- arbitrary executable embeds or author-supplied playback code; or
- renderer-specific DOM, audio element, decoded-buffer, or graphics state in
  canonical content.
