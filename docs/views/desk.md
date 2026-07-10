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

## Timer pill

Located centre-top of the canvas. Controls:
- **Wipe now** — clears the current canvas (archived first)
- **History** — browse past archives (read-only, amber border + banner)
- **Frequency** — how often the canvas auto-archives (default: 24h)

Archives saved to: `Scratchpad/Archive/YYYY-MM-DD-HHmm.canvas`  
SQLite indexes archives — visible in Sort with "scratchpad archive" label.

## Canvas storage

Desk canvas stored as JSON Canvas format. See ADR-006.

## Reference wireframe

<img src="/fosta/assets/wireframes/desk.svg" alt="desk wireframe" style="width:100%;border:1px solid #302825;border-radius:6px;margin:1rem 0">

*Reference wireframe — scratchpad canvas with freeform nodes, note-reference UUID cards, timer pill, and persistent capture bar. Recreated from early hand sketches, June 2026 — reference only, not final UI.*

