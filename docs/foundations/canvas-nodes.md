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
| Freeform | Ephemeral or permanent text content, not yet a note. Not a reference to anything. | Desk, and Track's Web layout only |
| Note-reference | UUID pointer to a real note in the library. Shows note title in compact state, full note content in expanded state. | Desk + Track (all layouts) |
| Canvas-reference | UUID pointer to a Track canvas file. Shows a live read-only preview of that Track canvas via tldraw. | Desk only |

Freeform nodes behave differently enough from the other two types that they're documented separately in full — see [Freeform nodes](#freeform-nodes) below.

**Freeform on Web, not Line or Thread:** freeform nodes were extended from Desk to Track's Web layout during hi-fi prototyping, since Web is Track's one free-form, unconstrained layout — see `docs/views/track.md` ("What it is") for the full reasoning. Line and Thread remain sequence-constrained and never carry freeform nodes; adding one to Line was tested and confirmed to have no sensible meaning (it simply appends into the sequence).

## Two-state model

Every note-reference and canvas-reference node exists in one of two states:

**Compact** — small card showing the title only. Acts as a quick link. The default state.

**Expanded live preview** — drag a corner handle past a size threshold and the node snaps into a live content preview that adapts continuously as you resize. Note-reference nodes show the note's body content. Canvas-reference nodes show the Track canvas rendered via tldraw (read-only).

The snap threshold creates a deliberate mode change — not a gradual degradation of a tiny card into unreadable text. Below the threshold: compact card. Above it: live preview.

**Freeform nodes do not participate in this model** — they are always shown as simple text boxes and cannot be expanded to a live preview, since there's no underlying note to preview. See [Freeform nodes](#freeform-nodes).

## Interaction model

The same interaction pattern applies to canvas nodes on both Desk and Track:

| Action | Result |
|---|---|
| Single click | Selects the node |
| Double click (compact or expanded header) | Opens Quick Look modal |
| Overflow menu (⋯) on hover | "Open in Quick Look" · "Open in Develop/Track" · "Add to Desk" |
| Drag corner handle past threshold | Snaps to expanded live preview state |
| "Open in Develop/Track" from Quick Look or overflow | Opens item in its full native view |

**Adding a node to an empty canvas (Desk, and Track's Web layout):** double-click or right-click empty canvas space opens a menu offering "Add freeform item" or "Add from library." Right-click was confirmed during hi-fi prototyping as an equivalent trigger to double-click and should be supported alongside it, not as a separate/lesser path.

## Relationship to Quick Look

Double-clicking any canvas node opens Quick Look — the floating preview modal. Quick Look is a separate Foundation; the canvas node interaction model is what triggers it. See [Quick Look](quick-look.md) for full documentation of the modal itself.

## Pinned nodes (Desk only)

Any canvas node on Desk — freeform, note-reference, or canvas-reference — can be marked **pinned**. Pinning is a property on the node, not a new node type or a new UI component.

**What pinning does:** a pinned node is excluded from Desk's timed wipe cycle (see `desk.md` — Timer pill). When the canvas wipes, the current state is archived as normal, non-pinned nodes clear from the live canvas, and pinned nodes remain exactly where they are, in whatever state they were in.

**What pinning does not change:** a pinned node behaves identically to an unpinned node of the same type in every other respect — same two-state model (where applicable), same editing behaviour, same interaction pattern. Pinning only affects wipe behaviour.

Pinning has a particular significance for Desk's freeform nodes specifically — see [Pinning and the third lifecycle option](#pinning-and-the-third-lifecycle-option) below.

**Pinning does not exist on Track's Web layout, and this is not an oversight.** Web has no wipe cycle at all — a Web canvas is permanent by default. Pinning is specifically the mechanism for protecting a node from a wipe; where there is no wipe, there is nothing to protect a node from, so the concept simply doesn't apply. Every node on Web is already "pinned" in effect, without needing the property.

### Why this is not a "panel"

An earlier proposal for persistent Focus/Pinned panels on Desk was explicitly rejected (see [Story](../project/story.md) — the pinned-nodes entry) on the grounds that a fixed UI region competes with the canvas and undermines Desk's core promise as a blank, chrome-free thinking surface.

Pinned nodes are not a panel. They are ordinary canvas content — positioned anywhere on the canvas by the user, rendered identically to any other node — that simply persists through the wipe cycle. There is no new docked region, no fixed sidebar, no dedicated UI surface. The distinction is deliberate: pinning changes *when a node is cleared*, not *where it lives or how it looks*.

## Freeform nodes

Freeform nodes are unanchored text content — not yet a note, not a reference to anything. They exist only on the canvas, with no underlying file. "Capture to Inbox" converts a freeform node in-place to a note-reference node, creating a real note in `Inbox/` and replacing the freeform node with a UUID card pointing to it.

Freeform nodes have no two-state model — they are always shown as simple text boxes, and cannot be expanded to a live preview, since there is no underlying note to preview.

**Two hosts, two lifecycles.** Freeform nodes exist in exactly two places, and the two behave differently:

| | Desk | Track — Web layout |
|---|---|---|
| Lifecycle | Ephemeral by default — cleared at the next wipe unless pinned | Permanent by default — no wipe cycle exists on Web at all |
| Pinning | A real, meaningful property (see Pinned nodes, above) | Does not exist — nothing to protect from a wipe that never happens |
| Everything else | Identical | Identical |

Aside from that lifecycle difference, freeform nodes behave identically wherever they appear — same editing, same timestamps, same "Capture to Inbox" conversion, same lack of a two-state model.

### Freeform node timestamps

Every freeform node silently records the moment it was first created — the instant the first character is typed — with no action required from the user. This is the node's **only** timestamp for as long as it remains freeform. Freeform nodes do not have a `modified` field; nothing about "last modified" is meaningful until the node becomes a tracked object in the system. This applies wherever a freeform node exists — Desk or Track's Web layout.

**Visual treatment:** the creation timestamp is not shown by default. It appears subtly — low-opacity, small — only on hover or when the node is selected. This preserves the canvas's blank, no-chrome feel while keeping the information available the moment a user wants to check it.

### What happens at conversion (Capture to Inbox)

When a freeform node is converted to a real note, the note's required frontmatter (`created`, `modified` — see Data Model) is populated as follows:

- **`created`** — backdated to the freeform node's original entry timestamp, not the moment of capture. The note's true origin is preserved: an idea jotted down three days ago and only captured today still shows `created: 3 days ago`.
- **`modified`** — set to the moment of capture, since converting the node into a real note is itself a genuine change to the object. From this point on, `modified` updates normally with every subsequent edit, exactly like any other note.

This means a freeform node's single timestamp becomes the note's `created` date, and the note only begins tracking `modified` from the moment it becomes a note — not before. The two fields are not parallel-tracked at different stages; `modified` simply does not exist until the frontmatter contract begins.

### Pinning and the third lifecycle option (Desk only)

By default, a freeform node on Desk has two possible fates: converted to a real note via "Capture to Inbox," or cleared at the next wipe.

Pinning a freeform node creates a third option: **content that stays freeform indefinitely, edited directly in place, and is never intended to become a note.** A user can type a running list, a set of quick reminders, or any short-lived-but-recurring text directly onto the canvas, pin it, and it persists across every wipe cycle without ever being captured to Inbox.

Pinning does not add a second timestamp. A pinned freeform node retains only its original creation date — there is no separate "pinned on" field.

This does not conflict with "everything is a note" (see Architecture Principles). Unpinned freeform nodes were already the one explicit exception to that rule — content that exists on the canvas without being a note. Pinning does not create a new exception; it extends the lifecycle of an existing one.

A freeform node on Track's Web layout has no equivalent third-option decision to make — it's permanent from the moment it's created, by default, with no wipe cycle to opt out of.
