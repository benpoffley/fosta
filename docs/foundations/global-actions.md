---
title: "Global Actions"
parent: "Foundations"
nav_order: 2
layout: default
---

# Global Actions

**Status:** Final  
**Type:** Foundations — not scoped to any single view

## What they are

Global Actions are capabilities tied to a UI element rather than a view. They work wherever that element is rendered, regardless of which view the user is in.

v1 implements two Global Actions. The broader principle — "create or send any item from any context where it makes sense" — is documented for future expansion but carries no open-ended v1 commitment.

## Add to Desk

Sends a note or Track canvas to the Desk scratchpad as a reference node.

- Creates a note-reference node (for notes) or canvas-reference node (for Track canvases)
- Placed at a random clear position on the scratchpad
- The original note or canvas doesn't move — only a reference is placed
- No new node types — these already exist on the scratchpad

### UI behaviour — context dependent

| Context | How it surfaces | Why |
|---|---|---|
| File navigator (Layer), Track canvas nodes, Desk reference nodes | Overflow menu (⋯) on hover — "Add to Desk" is one option | Note action sets will grow; hover-revealing on every item in a dense list would be noisy |
| Quick Look modal, Layer editor | Direct button at the top of the interface | Space exists to surface it directly |

## Create Track from tag

Hovering any tag, anywhere it renders — Sort's tag bubbles, inline tag chips in Layer's metadata row, block-level tags — surfaces a "Create Track" action.

Opens Track's creation modal pre-filled with the hovered tag as the first tag in a Live population combination. The user can add further tags to the combination, choose the layout (Line or Thread — the two layouts that support Live population), or switch to Manual before confirming.

### UI behaviour

- **Desktop:** hover reveals the action directly on the tag element
- **Mobile/tablet (future):** long press is the equivalent trigger

Tags use direct hover-reveal (not overflow) because they have exactly one action and are small, densely-packed elements where a permanent ⋯ affordance would crowd the label.

## Why the two patterns are different

Notes use overflow because their action set is likely to grow. Hover-revealing actions on every note in a dense list or canvas would be noisy.

Tags use direct hover-reveal because they are small, deliberately targeted elements with a single action.
