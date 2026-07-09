---
title: "Data Model"
parent: "Engineering"
nav_order: 3
layout: default
---

# Data Model

**Status:** Final

## Note structure

Every piece of content in Fosta is a note. Notes are Markdown files with YAML frontmatter.

### Standard note

```markdown
---
id: 550e8400-e29b-41d4-a716-446655440000
created: 2026-04-24T10:30:00Z
modified: 2026-04-24T10:30:00Z
tags: [filmmaking, ideas]
folder: Projects/Documentary
references: []
---

Note content in Markdown here.

Each paragraph is a block. <!-- id: a1b2c3d4 -->
```

### Note types

Type is expressed in frontmatter. Standard notes have no `type` field.

**Comment note**
```yaml
type: comment
parentNote: <uuid-of-parent-note>
parentBlock: <uuid-of-block-being-commented-on>
```

**Quote / transclusion**
```yaml
type: quote
sourceNoteId: <uuid>
sourceBlocks:
  - blockId: <uuid>
    charStart: 0
    charEnd: 142
snapshot_text: "Fallback text if source is unavailable"
```

**Share document**
```yaml
type: share
published: false
```

## Block UUIDs

Every Tiptap block has a stable UUID serialised as an HTML comment inline in the Markdown. Created once, never regenerated.

```markdown
This is a paragraph. <!-- id: 550e8400 tags: ["research"] -->

A heading block. <!-- id: a1b2c3d4 -->
```

**Critical:** If the Tiptap serialisation logic doesn't explicitly preserve node attributes through save/load cycles, block UUIDs will be silently lost. This must be explicitly handled and tested.

## Wikilinks

```markdown
[[display-name|uuid]]
```

Display name is human-readable in the raw Markdown file. UUID is the actual reference — survives renames. SQLite indexes these for reverse lookup (backlinks).

## Canvas files

Canvas files (Desk scratchpad, Track canvases) are stored as JSON Canvas format:

```json
{
  "nodes": [
    {
      "id": "node-uuid",
      "type": "note-reference",
      "noteId": "550e8400-e29b-41d4-a716-446655440000",
      "x": 100,
      "y": 200,
      "width": 200,
      "height": 150,
      "state": "compact"
    }
  ],
  "edges": []
}
```

Node types: `freeform`, `note-reference`, `canvas-reference`.
Node states: `compact` (title card), `expanded` (live preview).


## Track canvas files

Track canvases (Line, Web, and Thread) all use the same JSON Canvas format on disk, extended with two additional top-level fields:

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
      "id": "node-uuid",
      "type": "note-reference",
      "noteId": "550e8400-e29b-41d4-a716-446655440000",
      "x": 100,
      "y": 200,
      "state": "compact"
    },
    {
      "id": "annotation-uuid",
      "type": "annotation",
      "anchorType": "node",
      "anchorId": "node-uuid",
      "text": "Revisit this after the shoot"
    }
  ],
  "connections": [
    {
      "id": "connection-uuid",
      "from": "node-uuid-a",
      "to": "node-uuid-b"
    }
  ]
}
```

- `layout`: `"line"`, `"web"`, or `"thread"` — set at creation, cannot change after
- `population.mode`: `"manual"` or `"live"`
- `population.tags`: present only when `mode: "live"` — fixed array, ANDed together
- `population.sort`: `"created"` or `"modified"` — present on `line` and `thread` (Web has no sort axis)
- `connections`: always exactly two node IDs (`from`/`to`). Web canvases may have `connections: []`
- Node type `annotation`: anchors via `anchorType` (`"node"` or `"connection"`) + `anchorId`. Never present without an anchor. Not valid on Thread.
- These are Fosta-specific extensions to the JSON Canvas spec. The spec allows extension fields — these do not conflict with the standard.

**Thread rendering note:** Thread uses the same JSON Canvas file as Line and Web, including `x`/`y` position fields on nodes. tldraw is not invoked to render Thread — the scroll UI reads the node list directly and ignores position fields. This keeps one storage model for all three Track layouts.

## SQLite schema (index layer)

SQLite is a queryable index over the Markdown files. Always rebuildable. Never the source of truth.

```sql
CREATE TABLE notes (
  id TEXT PRIMARY KEY,
  path TEXT NOT NULL,
  title TEXT,
  created TEXT NOT NULL,
  modified TEXT NOT NULL,
  type TEXT,
  published INTEGER DEFAULT 0
);

CREATE TABLE note_tags (
  note_id TEXT NOT NULL,
  tag TEXT NOT NULL,
  PRIMARY KEY (note_id, tag)
);

CREATE TABLE wikilinks (
  source_note_id TEXT NOT NULL,
  target_note_id TEXT NOT NULL,
  display_name TEXT
);

CREATE TABLE transclusions (
  id TEXT PRIMARY KEY,
  source_note_id TEXT NOT NULL,
  target_note_id TEXT NOT NULL,
  source_blocks JSON NOT NULL,
  snapshot_text TEXT NOT NULL,
  created TEXT NOT NULL
);

CREATE TABLE blocks (
  id TEXT PRIMARY KEY,
  note_id TEXT NOT NULL,
  position INTEGER NOT NULL
);
```

## Tag model

Tags are a flat, single-level array in frontmatter:

```yaml
tags: [filmmaking, ideas, documentary]
```

No hierarchy. No nesting. Tag frequency is computed from SQLite at query time, not stored. Any UI differentiation (pinned tags, sizing in Sort) is a UI-layer concern only.
