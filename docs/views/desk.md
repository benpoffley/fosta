---
title: "Desk / Base"
parent: "Views"
nav_order: 1
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

## Node types

Three node types live on the scratchpad canvas. See [Canvas Nodes](../foundations/canvas-nodes.md) for the full model including two-state behaviour and interaction patterns.

- **Freeform** — ephemeral text, not yet a note. "Capture to Inbox" converts it in-place to a note-reference node.
- **Note-reference** — UUID card pointing at a real note. Created via Capture to Inbox or Add to Desk.
- **Canvas-reference** — UUID pointer to a Track canvas. Renders a live read-only tldraw preview. Not editable from Desk.

## Canvas node model

Node types, two-state behaviour (compact card / expanded live preview), and interaction patterns are documented in [Canvas Nodes](../foundations/canvas-nodes.md) — the full model applies here.
## Interaction model

| Action | Result |
|---|---|
| Single click on node | Selects the node |
| Double click on compact node | Opens Quick Look modal |
| Double click on expanded node header | Opens Quick Look modal |
| Overflow (⋯) on hover | "Open in Quick Look" · "Open in Layer/Track" · "Add to Desk" |
| "Open in Layer/Track" from Quick Look | Opens in full native view |

## Pinned nodes

Any node on the canvas — freeform, note-reference, or canvas-reference — can be pinned. Pinned nodes are excluded from the timer's wipe cycle and remain on the canvas indefinitely, in place, until unpinned or removed manually.

This is a property on the node, not a new component or panel — pinned nodes look and behave exactly like their unpinned counterparts in every respect except wipe behaviour. See [Canvas Nodes — Pinned nodes](../foundations/canvas-nodes.md#pinned-nodes-desk-only) for full reasoning, including why this is distinct from the previously-rejected Focus/Pinned panel concept.

**Common use:** a freeform node containing a running list or set of quick reminders, pinned so it survives every wipe without ever being converted to a real note. Edited directly in place — no Quick Look required.

## Timer pill

Located centre-top of the canvas. Controls:
- **Wipe now** — clears the current canvas (archived first). Pinned nodes are excluded and remain on the live canvas.
- **History** — browse past archives (read-only, amber border + banner)
- **Frequency** — how often the canvas auto-archives (default: 24h)

Archives saved to: `Scratchpad/Archive/YYYY-MM-DD-HHmm.canvas`  
SQLite indexes archives — visible in Sort with "scratchpad archive" label.

Archives capture the full canvas state at the moment of wipe, including pinned nodes, even though pinned nodes also remain live. This means a pinned node's state at any past wipe is always recoverable from history, even as its live version continues to change.

## Canvas storage

Desk canvas stored as JSON Canvas format. See ADR-006.

## Reference wireframe

<img src="/fosta/assets/wireframes/desk.svg" alt="desk wireframe" style="width:100%;border:1px solid #302825;border-radius:6px;margin:1rem 0">

*Reference wireframe — scratchpad canvas with freeform nodes, note-reference UUID cards, timer pill, and persistent capture bar. Recreated from early hand sketches, June 2026 — reference only, not final UI.*

