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

## How Quick Look is triggered

Quick Look is opened by double-clicking any canvas node (compact or expanded) on Desk or Track, or by clicking a wikilink within a note. The full interaction model — including single click, double click, and overflow menu behaviour across all contexts — is documented in [Canvas Nodes](canvas-nodes.md).

**File navigators** (Layer and Share) open directly without Quick Look — single click opens the note in the editor or preview pane respectively.

## Canvas node two-state model

Canvas nodes exist in compact and expanded live preview states — double-clicking either state opens Quick Look. See [Canvas Nodes](canvas-nodes.md) for the full node model and interaction pattern.

## Share — explicitly excluded

Quick Look is not available in Share. Share's middle preview pane already serves this function. Applying Quick Look there would be redundant.

## Architecture note

Entirely a UI layer concern. No new data model, no new note types, no new storage calls. Note variant uses existing `saveNote()`. Track canvas variant uses tldraw read-only with editing affordances disabled.
