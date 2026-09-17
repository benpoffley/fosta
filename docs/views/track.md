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

A tool for named, persistent arrangements of notes, distinct from Desk's disposable scratch space. Three layout modes, all working with note references.

**Not all three layouts are sequence-constrained.** Line and Thread are deterministic — notes go into a sequence or a query, with no free positioning. Web is the exception: a genuinely free-form canvas, deliberately mirroring Desk's capabilities (see Web, below). What makes something belong in Track is not "constrained," but **persistence and naming** — a Track is created, named, and kept; Desk is disposable by design and wipes on a timer. Web is a Track because it's a permanent, named thing you deliberately build and return to, not because it shares Line and Thread's sequence discipline. This was clarified during hi-fi prototyping, when building Web out fully surfaced that it had never actually been sequence-constrained — the original "Explicitly NOT a free-form canvas" framing was only ever true of Line and Thread.

## Three layout modes

Layout mode is chosen at creation and cannot be changed after.

### Line (formerly "Linear")

- Sequence axis (horizontal)
- Manual or Live tag-combination population (see Population model below)
- Sort by created or modified date
- Feeds Share — a Line Track is the primary source for Share composition
- **No freeform items.** Confirmed during hi-fi prototyping: adding freeform text to a Line track has no sensible meaning — it just gets appended into the sequence, which isn't a real use case. Line only ever contains note-reference nodes.

### Web (formerly "Spatial")

- Free-positioned, mindmap-style
- Manual population only — no Live/tag-query population
- No sequence axis
- **Mirrors Desk's full node capabilities** — note-reference nodes, canvas-reference nodes, connections, annotations, and **freeform items** (unanchored, freely-positioned text, capturable to a real note via "Capture to Inbox," identical to Desk — see [Canvas Nodes — Freeform nodes](../foundations/canvas-nodes.md#freeform-nodes)). The one thing Web does *not* have is Desk's wipe cycle — a Web canvas is permanent by default, with no timer, no archiving, and therefore no need for Desk's "pinning" concept either: nothing threatens a Web node's persistence, so there's nothing to protect it from.
- **Entry point:** double-click or right-click empty canvas opens a menu with two options — "Add freeform item" or "Add from library." Right-click working as an equivalent trigger to double-click was confirmed during hi-fi prototyping and is worth carrying into the real interaction spec, not just double-click.

### Thread (new)

- Full note content stacked vertically, scrollable — not a canvas, no tldraw rendering involved
- Shares Line's population model: manual or Live tag-combination, sorted by created/modified
- No connections, no annotations, **no freeform items** — note-reference nodes only, by design. This is a type-level restriction: there is no spatial or sequential structure for a connection, annotation, or freeform item to anchor to (or be positioned within) in a scroll
- Read-only in v1 — clicking a note opens it in Quick Look/Develop to edit. Inline editing (editing note content directly within the Thread scroll) is a v1.1 candidate — it would require live editor instances per note in the stack rather than static rendered content
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

- **Manual tracks (any layout):** navigator panel shown, matching the Develop/Share shell. Notes can be dragged directly onto the canvas/timeline/scroll, alongside the "Add new item" search modal.
- **Live tracks (any layout):** no file navigator. Only item-creation action is Create new.

### Content — combined Tracks + Notes panel

The navigator is one combined panel, resolved after hi-fi prototyping: a **Tracks** section (click any Track to open it as a new tab — see [View State Persistence](../foundations/view-state-persistence.md) for tab behaviour) sits above a **Notes** section (search, with a "+" to add the result to the currently active Manual track).

**Untagged Inbox notes are hidden from the Notes section's default list, but remain findable via its search** — the same browsable-vs-reachable rule applied in Develop's navigator and Share's library panel. See `docs/views/develop.md` for the full reasoning. This means "Add new item" search can still surface an untagged note if a user deliberately searches for it, even though it won't appear by default.

## Node types

### Note-reference nodes

UUID pointer to a note. On Line and Web, supports the two-state model (compact card / expanded live preview) — see [Canvas Nodes](../foundations/canvas-nodes.md) for the full model. On Thread, rendered as full note content in a scroll, not a card.

### Freeform nodes (Web only)

Unanchored, freely-positioned text — not a note, not a reference to anything, identical to Desk's freeform nodes. See [Canvas Nodes — Freeform nodes](../foundations/canvas-nodes.md#freeform-nodes) for the full model, including timestamps and "Capture to Inbox" conversion. **Not available on Line or Thread** — see those sections above for why.

### Annotation nodes (Line and Web only)

Freeform text annotations. Must anchor to exactly one target — a single node (rides with it) or a single connection. Cannot be freestanding. **Not available on Thread.**

Annotations are a distinct concept from freeform nodes (above): an annotation must anchor to something and cannot exist independently; a freeform node is unanchored and stands on its own. Both can appear on Web; only annotations can appear on Line.

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
| Right-click / Double-click on empty canvas (Web only) | Opens "Add freeform item" / "Add from library" menu | — |
| Overflow (⋯) | "Open in Quick Look" / "Open in Develop" / "Remove from Track" | "Open in Quick Look" / "Open in Develop" / "Remove from Track" |

All note-reference and canvas-reference nodes are read-only previews. No inline editing of note content in any mode in v1. Freeform nodes (Web only) are directly editable in place, identical to Desk.

## Canvas storage

Line and Web canvases stored as JSON Canvas format, rendered by tldraw. Thread uses the same JSON Canvas format on disk but tldraw is not invoked — the scroll UI reads the node list directly. See [ADR-006](../engineering/decisions/adr-006-json-canvas.md) and [ADR-011](../engineering/decisions/adr-011-tldraw-renderer-only.md).

The base canvas and node schema (`nodes`, `edges`, node types, `state`) lives in [Data Model — Canvas files](../engineering/data-model.md#canvas-files). Track extends it with additional top-level fields and a Track-only `annotation` node type:

```json
{
  "layout": "line",
  "population": {
    "mode": "live",
    "tags": ["research", "documentary"],
    "sort": "created"
  },
  "nodes": [
    {
      "id": "annotation-uuid",
      "type": "annotation",
      "anchorType": "node",
      "anchorId": "node-uuid",
      "text": "Revisit this after the shoot"
    }
  ],
  "connections": [
    { "id": "connection-uuid", "from": "node-uuid-a", "to": "node-uuid-b" }
  ]
}
```

- `layout`: `"line"`, `"web"`, or `"thread"` — set at creation, cannot change
- `population.mode`: `"manual"` or `"live"`
- `population.tags`: present only when `mode: "live"` — fixed array, ANDed together
- `population.sort`: `"created"` or `"modified"` — Line and Thread only
- `annotation` nodes: freeform text anchored to exactly one node or connection — Line and Web only
- `freeform` nodes: unanchored text, Web only — same schema as Desk's freeform nodes (see [Data Model](../engineering/data-model.md))
- `connections`: always exactly two node IDs. These are Fosta-specific extensions to the JSON Canvas spec.
- **Thread has no canvas file** — it renders the same population object as a scrollable list with no tldraw dependency.

## Reference wireframe

<img src="/fosta/assets/wireframes/track.svg" alt="track wireframe" style="width:100%;border:1px solid #302825;border-radius:6px;margin:1rem 0">

*Reference wireframe — Line / Web / Thread layout modes. Annotations anchored to nodes or connections. Recreated from early hand sketches, June 2026 — reference only, not final UI.*
