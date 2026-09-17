---
title: "Develop"
parent: "Views"
nav_order: 3
layout: default
---

# Develop

**Status:** Decided — Final  
**Name:** Develop. Confirmed final during hi-fi prototyping — "Layer" failed the CTA test ("Open in Layer" read oddly), and after working with a real prototype under the name "Develop," it was confirmed as the better fit.

## What it is

The main editor view. Deep work — block-typed editing, backlinks, comments, metadata. Where ideas are developed into real content.

## Layout — three panels

| Panel | Contents | Collapsible? |
|---|---|---|
| Left | File navigator — vault browser | Yes |
| Centre | Block-typed Tiptap editor with tab bar | No |
| Right | Layers panel — metadata + backlinks (top), block comments + tags (bottom, when block selected) | Yes |

## File navigator (left panel)

Vault browser. Single-click opens a note directly in the editor. Double-click does nothing.

**Flat, tag-based organisation — no folders.** Fosta dropped folder-based organisation early in favour of a flat tag system (see [Architecture Principles](../engineering/architecture.md) — "Everything is a note," and the data model's `tags: []` array with no folder field). This was revisited during hi-fi prototyping — a working prototype briefly reintroduced folders (Inbox/Treatment/Reference/Admin-style groups) purely as a prototyping default, not a deliberate reversal — and the flat-tag decision was reaffirmed. **Folders do not exist anywhere in Fosta's data model or UI.** Noted explicitly here so this doesn't quietly resurface in a future prototype without this context.

### Inbox notes — hidden from browsing, findable by search

Untagged notes sitting in Inbox do **not** appear in the navigator's default browsing list. This isn't a data-model exclusion — an Inbox note is a full note the moment it's captured, same as any other — it's a *usefulness* distinction: an untagged note has no organised place to sit in a browsing list, no tag to file it under, nothing meaningful to do with it yet in Develop, Track, or Share. Showing it in the default list would just be noise ahead of the moment it actually becomes useful.

**Search is the exception.** Untagged Inbox notes remain fully findable via search within Develop (and within Track's "Add new item" search, and Share's library panel search) — search reflects "I know what I'm looking for," a different intent than browsing, and hiding real content from a deliberate search would be actively unhelpful.

The general rule, stated once for reuse: **browsable lists show organised (tagged) material by default; search reaches everything, tagged or not.** This applies consistently across Develop's navigator, Track's "Add new item" search, and Share's library panel — see `docs/views/track.md` and `docs/views/share.md` for the same rule applied there.

Sort is the deliberate exception to this rule, not an inconsistency: Sort's entire purpose is showing the unsorted pile, so Inbox notes are the main event there, never hidden.

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
| File navigator (Develop left panel) | ❌ No | Would clutter the vault browser with entries that aren't "notes" in the user's mental model — a note with 20 comments shouldn't produce 20 extra navigator rows |
| Layers panel, block selected (Develop right panel) | ✅ Yes | This is the canonical, intended access point — comments are accessed through their parent note, not browsed independently |
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

"Add to Desk" is available as a direct button in the Develop editor (not buried in overflow). See `../foundations/global-actions.md`.

## Reference wireframe

<img src="/fosta/assets/wireframes/layer.svg" alt="develop wireframe" style="width:100%;border:1px solid #302825;border-radius:6px;margin:1rem 0">

*Reference wireframe — three-panel editor with file navigator, block-typed editor, and layers panel. Recreated from early hand sketches, June 2026 — reference only, not final UI. (Wireframe filename predates the Develop naming decision.)*
