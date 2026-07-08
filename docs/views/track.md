---
title: "Track"
parent: "Views"
nav_order: 5
layout: default
---

# Track

**Status:** Decided — Provisionally v1  
**Note:** Will be reassessed after Capture + Sort are in daily use. May move to v1.1.

## What it is

A constrained timeline and relationship tool. Explicitly NOT a free-form canvas — it is distinct from Obsidian-style canvases. Two layout modes, both working with note references.

## Two layout modes

Layout mode is chosen at creation and cannot be changed after.

### Linear mode
- Sequence axis (horizontal)
- Manual or live tag-query population
- Sort by created or modified date
- Feeds Share — a Linear Track is the primary source for Share composition

### Spatial mode
- Free-positioned, mindmap-style
- Manual population only
- No sequence axis

## Node types

### Note-reference nodes
UUID pointer to a note. Supports the two-state model (compact card / expanded live preview). See `desk.md` for the two-state model — it is identical here.

### Annotation nodes
Freeform text annotations. Must anchor to exactly one target:
- A single node (rides with it when it moves)
- A single connection

Annotations cannot be freestanding/floating. No independent position.

## Connections

- Always between exactly two nodes
- Visual-only — no relationship data structure, no graph link model
- Each connection has a stable ID
- Annotations can anchor to a connection

## Adding notes to a Track

Via "Add new item" affordance — opens a search/select modal. No file navigator panel in Track. The user searches their vault, selects a note, it's added as a reference node.

## Canvas storage

Track canvases stored as JSON Canvas format. tldraw renders them. See ADR-006, ADR-011.

## Interaction model

Identical to Desk canvas:
- Single click = select
- Double click = Quick Look
- Overflow (⋯) = "Open in Quick Look" / "Open in Layer"
