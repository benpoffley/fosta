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

## Pinned nodes (Desk only)

Any canvas node on Desk — freeform, note-reference, or canvas-reference — can be marked **pinned**. Pinning is a property on the node, not a new node type or a new UI component.

**What pinning does:** a pinned node is excluded from Desk's timed wipe cycle (see `desk.md` — Timer pill). When the canvas wipes, the current state is archived as normal, non-pinned nodes clear from the live canvas, and pinned nodes remain exactly where they are, in whatever state they were in.

**What pinning does not change:** a pinned node behaves identically to an unpinned node of the same type in every other respect — same two-state model, same editing behaviour, same interaction pattern. Pinning only affects wipe behaviour.

### Why this matters for freeform nodes specifically

Freeform nodes are ephemeral by default — text typed directly onto the canvas with no underlying note, intended to either be converted to a real note via "Capture to Inbox" or cleared at the next wipe.

Pinning a freeform node creates a third lifecycle option: **content that stays freeform indefinitely, edited directly in place, and is never intended to become a note.** A user can type a running list, a set of quick reminders, or any short-lived-but-recurring text directly onto the canvas, pin it, and it persists across every wipe cycle without ever being captured to Inbox.

This does not conflict with "everything is a note" (see Architecture Principles). Unpinned freeform nodes were already the one explicit exception to that rule — content that exists on the canvas without being a note. Pinning does not create a new exception; it extends the lifecycle of an existing one.

### Why this is not a "panel"

An earlier proposal for persistent Focus/Pinned panels on Desk was explicitly rejected (see Decisions Explicitly Rejected, `docs/project/scope.md` or `current-development.md`) on the grounds that a fixed UI region competes with the canvas and undermines Desk's core promise as a blank, chrome-free thinking surface.

Pinned nodes are not a panel. They are ordinary canvas content — positioned anywhere on the canvas by the user, rendered identically to any other node — that simply persists through the wipe cycle. There is no new docked region, no fixed sidebar, no dedicated UI surface. The distinction is deliberate: pinning changes *when a node is cleared*, not *where it lives or how it looks*.

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
