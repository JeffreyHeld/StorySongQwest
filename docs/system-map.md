# System Map - Supporting Dependency Graph

This document is a compact supporting diagram for the StorySongQwest workspace.
It is subordinate to `docs/architecture-map.md`, which is the canonical source
for workspace architecture, repository ownership, feature routing, and promotion
ownership.

Use this system map for a fast visual summary of the dependency flow. If this
document conflicts with `docs/architecture-map.md`, update this document or
revise the architecture map first.

```text
Phase Specs
Phase 0–7 product and platform intent
            │
            ▼
qwest-design-system
Experience Lab
            │
            ▼
Prototype
reader, listener, authoring,
gallery
      │                 │
      │                 ▼
      │        SongQwest Listener /
      │        Timeline Prototype
      │                 │
      └────────┬────────┘
               ▼
Promotion Audit
what is ready, missing,
duplicated
               │
               ▼
Runtime Promotion
Roadmap
promotion rules and levels
               │
               ▼
Runtime Milestones
execution sequence
        │                     │
        ▼                     ▼
story-song-qwest      SongQwest Runtime
production runtime    listener, lyric,
                      audio, timeline
        │                     │
        └────────┬────────────┘
                 ▼
Published Bundle
portable qwest output
                 │
                 ▼
Reader Runtime
StoryQwest consumer
experience
```

## Development Flow

1. **Phase Specs**
   - Phase 0–7 product and platform intent

2. **qwest-design-system**
   - Experience Lab

3. **Prototype**
   - Reader
   - Listener
   - Authoring
   - Gallery

4. **SongQwest Listener / Timeline Prototype**
   - Parallel prototype feeding into the promotion process.

5. **Promotion Audit**
   - What is ready
   - What is missing
   - What is duplicated

6. **Runtime Promotion Roadmap**
   - Promotion rules
   - Promotion levels

7. **Runtime Milestones**
   - Execution sequence

8. **Production Runtime**
   - StoryQwest production runtime
   - SongQwest runtime (listener, lyric, audio, timeline)

9. **Published Bundle**
   - Portable Qwest output

10. **Reader Runtime**
    - StoryQwest consumer experience
