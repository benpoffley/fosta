---
title: "Canvas Nodes"
parent: "Foundations"
nav_order: 4
layout: default
---

# Canvas Nodes

**Status:** Final  
**Type:** Foundations — applies to Desk and Track canvases

Canvas nodes are the building blocks of Fosta's two canvas views — Desk and Track. The node model, states, and interaction patterns are identical across both views.

## Three node types

| Type | Description | Views |
|---|---|---|
| Freeform | Ephemeral text content, not yet a note. Not a reference to anything. | Desk only |
| Note-reference | UUID pointer to a real note in the library. Shows note title in compact state, full note content in expanded state. | Desk + Track |
| Canvas-reference | UUID pointer to a Track canvas file. Shows a live read-only preview of that Track canvas via tldraw. | Desk only |

## Two-state model

Every note-reference and canvas-reference node exists in one of two states:

**Compact** — small card showing the title only. Acts as a quick link. The default state.

**Expanded live preview** — drag a corner handle past a size threshold and the node snaps into a live content preview that adapts continuously as you resize. Note-reference nodes show the note's body content. Canvas-reference nodes show the Track canvas rendered via tldraw (read-only).

The snap threshold creates a deliberate mode change — not a gradual degradation of a tiny card into unreadable text. Below the threshold: compact card. Above it: live preview.

## Interaction model

The same interaction pattern applies to canvas nodes on both Desk and Track:

| Action | Result |
|---|---|
| Single click | Selects the node |
| Double click (compact or expanded header) | Opens Quick Look modal |
| Overflow menu (⋯) on hover | "Open in Quick Look" · "Open in Layer/Track" · "Add to Desk" |
| Drag corner handle past threshold | Snaps to expanded live preview state |
| "Open in Layer/Track" from Quick Look or overflow | Opens item in its full native view |

## Relationship to Quick Look

Double-clicking any canvas node opens Quick Look — the floating preview modal. Quick Look is a separate Foundation; the canvas node interaction model is what triggers it. See [Quick Look](quick-look.md) for full documentation of the modal itself.

## Freeform nodes (Desk only)

Freeform nodes are ephemeral text content on the Desk scratchpad — not yet a note, not a reference. They exist only on the canvas. "Capture to Inbox" converts a freeform node in-place to a note-reference node, creating a real note in `Inbox/` and replacing the freeform node with a UUID card pointing to it.

Freeform nodes have no two-state model — they are always shown as simple text boxes. They cannot be expanded to a live preview because they have no underlying note to preview.
