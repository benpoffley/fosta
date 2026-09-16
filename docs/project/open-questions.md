---
title: "Open Questions"
parent: "Project"
nav_order: 4
layout: default
---

# Open Questions

Items that are genuinely unresolved. Not deferred — actively open.

## Three view names

Deferred to hi-fi Figma phase:
- Desk vs Base
- Sort vs Curate
- Layer vs Work vs Develop

## Quick Look wikilink stack — stress testing needed

The back-arrow navigation stack within Quick Look needs testing across edge cases:
- Deeply nested wikilinks (A → B → C → D...)
- Broken/missing references
- Circular links (A → B → A)
- Notes that link to themselves

## Track file navigator — what does it browse?

Whether Track's file navigator (when open) shows notes (for dragging into Manual tracks, matching Layer's pattern) or shows other Track canvases (for quickly switching between open Tracks without leaving the view). Three options were sketched — navigator shows Tracks only, a two-mode Notes/Tracks toggle, or a separate quick-switcher UI distinct from the navigator entirely — but none was chosen. See `docs/foundations/view-state-persistence.md` for the navigator-visibility rules that apply once this is decided.

## Track in v1.0 or v1.1?

Track is provisionally v1.0, but this will be reassessed after Capture + Sort are built and in daily use. If they take longer than expected or reveal fundamental questions about the data model, Track moves to v1.1.

## Capture keyboard shortcut

⌘N or similar — TBD. Not yet decided whether this opens a new note in Layer or triggers the Capture bar.

## Payment provider

Paddle vs Lemon Squeezy — not yet decided. Both support indie Mac apps. Decision deferred until closer to launch.
