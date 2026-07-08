---
title: "Desk / Base"
parent: "Views"
nav_order: 2
layout: default
---

# Desk / Base

**Status:** Decided  
**Name:** TBD — candidates: Desk, Base

## What it is

The home base and thinking space. A full-canvas scratchpad with floating UI on top. No side panels. The user works freely with ideas before they enter the pipeline.

## Layout

- Full canvas (tldraw) fills the space
- Timer pill at centre-top
- Persistent capture bar at centre-bottom, above the toolbar
- No side panels of any kind

## Three node types

### Freeform nodes
Ephemeral text content. Not yet a note. User can type freely. "Capture to Inbox" converts a freeform node in-place to a note-reference node and creates the note in `Inbox/`.

### Note-reference nodes
A UUID card pointing at a real note in the library. Created by:
- "Capture to Inbox" on a freeform node (converts in-place)
- "Add to Desk" from any other view or Quick Look

### Canvas-reference nodes
A UUID pointer to a Track canvas file. Renders a live, read-only preview of that Track canvas using tldraw. Not editable from Desk.

## Canvas node two-state model

Both note-reference and canvas-reference nodes exist in two states:

| State | Appearance | Trigger |
|---|---|---|
| Compact | Small card showing title only — acts as a quick link | Default |
| Expanded live preview | Full content preview that adapts as you resize | Drag corner handle past size threshold — snaps into live content mode |

The snap threshold creates a deliberate mode change, not a gradual degradation. Applies consistently across Desk and Track canvases.

## Interaction model

| Action | Result |
|---|---|
| Single click on node | Selects the node |
| Double click on compact node | Opens Quick Look modal |
| Double click on expanded node header | Opens Quick Look modal |
| Overflow (⋯) on hover | "Open in Quick Look" · "Open in Layer/Track" · "Add to Desk" |
| "Open in Layer/Track" from Quick Look | Opens in full native view |

## Timer pill

Located centre-top of the canvas. Controls:
- **Wipe now** — clears the current canvas (archived first)
- **History** — browse past archives (read-only, amber border + banner)
- **Frequency** — how often the canvas auto-archives (default: 24h)

Archives saved to: `Scratchpad/Archive/YYYY-MM-DD-HHmm.canvas`  
SQLite indexes archives — visible in Sort with "scratchpad archive" label.

## Canvas storage

Desk canvas stored as JSON Canvas format. See ADR-006.
