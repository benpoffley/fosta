---
title: "Quick Look"
parent: "Foundations"
nav_order: 3
layout: default
---

# Quick Look

**Status:** Final  
**Type:** Foundations — available from any view

## Canonical term

Quick Look is the definitive name for this pattern. Do not use:
- "expanded capture card" ❌
- "preview modal" ❌  
- "full quick-capture card" ❌

Use Quick Look consistently in copy, documentation, and code.

## What it is

A focused floating surface that lets users preview and interact with a note or Track canvas without leaving their current view. It is a Foundations-level capability — not scoped to any single view.

The Capture expanded state is Quick Look — they are the same surface.

## Two variants

### Note variant — editable
- Shows title and body only
- No right panel (no metadata, backlinks, or block comments — those are Layer's domain)
- Autosaves immediately via `saveNote()` on every change
- "Add to Desk" as a direct button at top
- "Open in Layer" escape hatch
- Reuses the expanded capture card component

### Track canvas variant — read-only
- Renders the Track canvas via tldraw in read-only mode
- All editing affordances disabled (no drag, no connect, no annotation tools)
- Subtle read-only indicator
- "Open in Track" escape hatch

## Wikilink navigation within Quick Look

Clicking a wikilink inside a note open in Quick Look opens the linked note within the same modal — not a second modal, not a navigation away.

- Back arrow (←) appears in modal header once there is history
- Session-only navigation stack — cleared when the modal is closed
- Stack is shallow — not persisted between Quick Look sessions

## Interaction model

| Context | Single click | Double click |
|---|---|---|
| Canvas node — compact card (Desk or Track) | Selects the node | Opens Quick Look |
| Canvas node header — expanded live preview | Selects the frame | Opens Quick Look |
| Canvas node overflow menu (⋯) | — | "Open in Quick Look" / "Open in Layer or Track" |
| Layer file navigator | Opens note in editor directly | — |
| Share file navigator | Opens note in preview pane directly | — |
| Wikilink within a note or Quick Look | Opens linked note in Quick Look (back stack) | — |
| Sort inbox grid | TBD — deferred | TBD — deferred |

## Canvas node two-state model

Canvas nodes exist in compact and expanded live preview states — double-clicking either state opens Quick Look. See [Canvas Nodes](canvas-nodes.md) for the full node model and interaction pattern.

## Share — explicitly excluded

Quick Look is not available in Share. Share's middle preview pane already serves this function. Applying Quick Look there would be redundant.

## Architecture note

Entirely a UI layer concern. No new data model, no new note types, no new storage calls. Note variant uses existing `saveNote()`. Track canvas variant uses tldraw read-only with editing affordances disabled.
