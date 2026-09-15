---
title: "Layer"
parent: "Views"
nav_order: 3
layout: default
---

# Layer / Work / Develop

**Status:** Decided  
**Name:** TBD — candidates: Layer, Work, Develop  
**Note:** "Layer" fails the CTA test — "Open in Layer" reads oddly.

## What it is

The main editor view. Deep work — block-typed editing, backlinks, comments, metadata. Where ideas are developed into real content.

## Layout — three panels

| Panel | Contents | Collapsible? |
|---|---|---|
| Left | File navigator — Obsidian-style vault browser | Yes |
| Centre | Block-typed Tiptap editor with tab bar | No |
| Right | Layers panel — metadata + backlinks (top), block comments + tags (bottom, when block selected) | Yes |

## File navigator (left panel)

Obsidian-style vault browser. Full folder operations (create, rename, move, delete). Same data as Sort — different activity. Single-click opens a note directly in the editor. Double-click does nothing.

## Editor (centre panel)

Tiptap block-typed editor. Features:
- Slash `/` command opens block type selector
- Tab bar for multiple open notes
- Every block has a stable UUID (never regenerated)
- Block UUIDs serialised as HTML comments: `<!-- id: 550e8400 -->`

## Layers panel (right panel)

**When no block selected:**
- Note metadata (title, tags, created, modified)
- Backlinks — notes that link to this note via wikilinks

**When a block is selected:**
- Block-level comments (notes with `type: comment`, `parentBlock: <uuid>`)
- Block-level tags

## Block-level comments

Comments are notes with `type: comment`, carrying `parentNote` and `parentBlock` references. Full comment schema: [Data Model](../engineering/data-model.md#note-types).

Anchored to the block UUID. If the block is moved within the note, the comment follows. If the block is deleted, the comment is orphaned (warning shown).

### Comments and the rest of the app — where they do and don't appear

Comments are full notes architecturally (UUID, frontmatter, disk file, SQLite-indexed) per "everything is a note." But they are content *bound to a specific block in a specific note*, not standalone thinking material, and are surfaced accordingly:

| Location | Comments appear? | Reasoning |
|---|---|---|
| File navigator (Layer left panel) | ❌ No | Would clutter the vault browser with entries that aren't "notes" in the user's mental model — a note with 20 comments shouldn't produce 20 extra navigator rows |
| Layers panel, block selected (Layer right panel) | ✅ Yes | This is the canonical, intended access point — comments are accessed through their parent note, not browsed independently |
| Global / Sort search | ✅ Yes | Comments are real content with real UUIDs; excluding them from search would hide genuine information from the user |
| Sort inbox / tag cloud | ❌ No | Comments are not standalone triage material — they don't carry independent tags and aren't meant to be sorted as if they were freestanding notes |
| Quick Look | ❌ No | Comments are not opened as a standalone note-editing surface; they're edited inline within the Layers panel |
| Track / Desk (as note-reference nodes) | ❌ No | Comments cannot be added to a canvas as a reference node — only standard notes can |

**The general rule:** comments are full notes for storage, indexing, and architectural purposes (this is what "everything is a note" guarantees — no parallel object type, no special-cased storage). But every *view* that presents "notes" to the user for browsing, tagging, or referencing treats comments as out of scope, since they are context-bound annotations rather than freestanding ideas. Search is the one exception, because comments are still real content a user may need to find.

## Wikilinks

Syntax: `[[display-name|uuid]]`

Clicking a wikilink opens the linked note in Quick Look modal (not directly in the editor). This lets the user reference without losing their place. Back arrow (←) in Quick Look navigates the stack.

## Split view

Deferred to v1.1.

## Add to Desk

"Add to Desk" is available as a direct button in the Layer editor (not buried in overflow). See `../foundations/global-actions.md`.

## Reference wireframe

<img src="/fosta/assets/wireframes/layer.svg" alt="layer wireframe" style="width:100%;border:1px solid #302825;border-radius:6px;margin:1rem 0">

*Reference wireframe — three-panel editor with file navigator, block-typed editor, and layers panel. Recreated from early hand sketches, June 2026 — reference only, not final UI.*

