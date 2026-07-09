---
title: "Track"
parent: "Views"
nav_order: 4
layout: default
---

# Track

**Status:** Decided — Provisionally v1
**Note:** Will be reassessed after Capture + Sort are in daily use. May move to v1.1.

## What it is

A constrained timeline and relationship tool. Explicitly NOT a free-form canvas — distinct from Obsidian-style canvases. Three layout modes, all working with note references.

## Three layout modes

Layout mode is chosen at creation and cannot be changed after.

### Line (formerly "Linear")

- Sequence axis (horizontal)
- Manual or Live tag-combination population (see Population model below)
- Sort by created or modified date
- Feeds Share — a Line Track is the primary source for Share composition

### Web (formerly "Spatial")

- Free-positioned, mindmap-style
- Manual population only — no Live/tag-query population
- No sequence axis

### Thread (new)

- Full note content stacked vertically, scrollable — not a canvas, no tldraw rendering involved
- Shares Line's population model: manual or Live tag-combination, sorted by created/modified
- No connections, no annotations — note-reference nodes only, by design. This is a type-level restriction: there is no spatial or sequential structure for a connection or annotation to anchor to in a scroll
- Read-only in v1 — clicking a note opens it in Quick Look/Layer to edit. Inline editing (editing note content directly within the Thread scroll) is a v1.1 candidate — it would require live editor instances per note in the stack rather than static rendered content
- Stored as JSON Canvas on disk (same format as Line and Web) — tldraw is not invoked for rendering. The scroll UI reads the node list directly, ignoring position fields

## Population model

Every Track (any layout) is either **Manual** or **Live**.

| | Manual | Live |
|---|---|---|
| Population | Notes added one at a time, stay until removed | Driven by a fixed AND-only combination of tags set at creation |
| Rule | Nothing added automatically | A note appears if and only if it carries every tag in the combination |
| Layouts | Line, Web, Thread | Line and Thread only (Web is manual-only) |
| File navigator | Shown | Hidden |
| "+ add item" | Available | Not available |
| Create new | Not available | Available (pre-tags with full combination) |

### Live tracks — key behaviours

**No "+ add item" action.** Population is query-driven. A manual add would be ambiguous — does the note get the tag automatically, or become an orphan? Removing the affordance removes the ambiguity.

**No file navigator.** Same reasoning — drag-to-add has no well-defined meaning against a query-driven set.

**Create new (Live only).** Creates a note pre-tagged with every tag in the Track's combination. Because the note is born fully tagged, it appears in the Track immediately (the query now matches it) and does not appear in Inbox. This is a display consequence only — no change to file routing. Tags are frontmatter metadata, not folder placement.

### Live → Manual conversion

One-way. Freezes current membership — notes in the Track at conversion become its fixed manual set. Future tag matches no longer auto-populate. Converting back to Live is not supported in v1; if added later it should preview what would be re-added before committing.

## File navigator

- **Manual tracks (any layout):** navigator panel shown, matching the Layer/Share shell. Notes can be dragged directly onto the canvas/timeline/scroll, alongside the "Add new item" search modal.
- **Live tracks (any layout):** no file navigator. Only item-creation action is Create new.

## Node types

### Note-reference nodes

UUID pointer to a note. On Line and Web, supports the two-state model (compact card / expanded live preview) — see `desk.md`, identical here. On Thread, rendered as full note content in a scroll, not a card.

### Annotation nodes (Line and Web only)

Freeform text annotations. Must anchor to exactly one target — a single node (rides with it) or a single connection. Cannot be freestanding. **Not available on Thread.**

## Connections (Line and Web only)

- Always between exactly two nodes
- Visual-only — no relationship data structure, no graph link model
- Each connection has a stable ID
- Annotations can anchor to a connection
- **Not available on Thread**

## Interaction model

| Action | Line + Web | Thread |
|---|---|---|
| Single click | Selects the node | Selects the note (enables remove via backspace or overflow) |
| Double click | Opens Quick Look | Opens Quick Look |
| Overflow (⋯) | "Open in Quick Look" / "Open in Layer" / "Remove from Track" | "Open in Quick Look" / "Open in Layer" / "Remove from Track" |

All nodes are read-only previews. No inline editing in any mode in v1.

## Canvas storage

Line and Web canvases stored as JSON Canvas format, rendered by tldraw. Thread uses the same JSON Canvas format on disk but tldraw is not invoked — the scroll UI reads the node list directly. See ADR-006, ADR-011.
