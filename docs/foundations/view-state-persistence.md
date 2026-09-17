---
title: "View State Persistence"
parent: "Foundations"
nav_order: 5
layout: default
---

# View State Persistence

**Status:** Decided — Final

Every view keeps its own state when the user switches away and back — the same way a serious creative tool like Photoshop or Figma doesn't reset your canvas position just because you switched panels. This page defines what persists, what doesn't, and how explicit navigation interacts with persisted state.

## The general rule

Each view remembers its own state independently, scoped to the current vault (see ADR-014), persisted across app restarts, not just within a session. Switching views doesn't destroy anything — the view you left sits dormant with its state intact until you return.

**The one exception is Sort.** Sort always resets to the Inbox tab, with search cleared and no filter or selection carried over, every time it's opened. This is deliberate, not an oversight: Sort is a *triage session*, not a workspace you return to mid-task. Each time you enter Sort, you're starting a fresh pass at whatever's currently unsorted — carrying over yesterday's search term or filter would work against that. Every other view is a workspace; Sort is a session you begin anew.

## What persists, per view

| View | What persists |
|---|---|
| Desk | Canvas pan position, zoom level, which nodes are expanded vs. compact |
| Sort | Nothing — always resets to Inbox tab, cleared search, no active filter (see above) |
| Layer | Which note is open, editor scroll position, panel widths (if resizable) |
| Track | Which tabs are open (see "Track tabs" below), each tab's own zoom/pan/scroll, whether the file navigator is open or closed |
| Share | Which Share document is open, which note is in the preview pane |

## Two categories of state — and how explicit navigation interacts with them

Per-view state splits into two categories that behave differently when the user explicitly navigates somewhere (e.g. "Open in Layer," "Create Track from tag," "Open in Track" from an overflow menu):

1. **"What's open"** — the active note in Layer, the active Track in Track, the active document in Share. **Explicit navigation always overrides this.** Clicking "Open in Layer" makes that specific note the open note in Layer, full stop.
2. **Everything else** — panel widths, scroll position, zoom/pan, tab structure, navigator open/closed state. **This persists regardless of how the view was arrived at.** Explicit navigation changes *what's* open, never *how the view looks or is configured*.

For example: clicking "Open in Layer" from a Quick Look switches to Layer, opens that specific note — but the file navigator's width, whether the layers panel is expanded, and any other configuration stay exactly as they were the last time Layer was used. The same principle applies to "Create Track from tag": Track becomes active, the newly created Track becomes the open tab, but Track's navigator open/closed state and any other open tabs are untouched.

## Track tabs

Track adopts the same tab pattern as Layer — multiple Tracks can be open simultaneously in tabs. Each open tab keeps its own state (zoom, pan, scroll position) independently while open.

**Navigator open/closed state is view-level, not tab-level.** Whether Track's file navigator is open or closed is a single setting shared across every tab, not scoped per-tab — switching tabs never causes the navigator to flicker open or closed. This follows directly from the "everything else persists independent of what's open" rule above: navigator visibility is configuration, not "what's open."

**What the Track navigator browses is a separate, still-open design question** — whether it shows notes (for dragging into Manual tracks, matching Layer's pattern) or shows other Track canvases (for quickly switching between Tracks without leaving the view). See Open Questions.

## Per-canvas zoom and pan — scoped to the canvas, not the view

Zoom and pan are properties of the specific canvas (a specific Track, or the Desk scratchpad) — not a shared "Track view" or "Desk view" setting. Each Track remembers its own zoom/pan independently, the same way each file in a design tool remembers its own view state. Switching between Tracks (via tabs or otherwise) restores each Track's own last-known zoom and pan, never a generic shared default.

## Default state when a tab is closed and later reopened

When a Track tab is closed and the same Track is opened again later, it does not resume from wherever it was left — it opens to a deliberate default, which differs by layout mode:

| Layout | Default on reopen |
|---|---|
| Web | Zoom-to-fit — shows every node regardless of count. Web has no sequence axis and no concept of "recent," so there's no more meaningful default than seeing everything at once. |
| Line | A fixed, standard zoom level — **not** zoom-to-fit, so density stays readable regardless of how many notes have accumulated over time — positioned at the end of the sequence corresponding to the most recent item, per whichever sort order is active. The zoom level itself does not adapt to node count or density; if a track has many notes, the user pans, the app never auto-shrinks to compensate. |
| Thread | Scrolled to the top of the list, respecting the active sort order — "top" is whichever item the current sort puts first. |

This gives one consistent mental model: closing and reopening a Track always shows a deliberate, predictable view — either the whole picture (Web) or the working end (Line) or the top of the list (Thread) — never a remembered arbitrary scroll or zoom position from whenever it happened to be closed.

## Fallback behaviour for deleted or missing content

If the note, Track, or document that was open in a view no longer exists when the view is returned to (deleted, moved, or otherwise missing), the view falls back to an empty state rather than erroring. This applies uniformly across Layer, Track, and Share.
