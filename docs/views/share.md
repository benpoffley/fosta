---
title: "Share"
parent: "Views"
nav_order: 5
layout: default
---

# Share

**Status:** Final

## What it is

The composition and publishing view. Where notes are assembled into a publishable document using live transclusions.

## Layout — three panels

| Panel | Contents |
|---|---|
| Left | File navigator — vault browser, single-click opens in preview pane |
| Middle | Preview/selection pane — open a note here to select content |
| Right | Share note — always visible, the composition being built |

## The Share document

A Share document is a note with `type: share` in frontmatter — not a separate document model. Full share schema: [Data Model](../engineering/data-model.md#note-types).

Content is composed of QuoteBlocks — live transclusions that resolve from SQLite at render time.

## Adding content: two methods

### Drag to quote
1. Open a note in the middle preview pane
2. Highlight text in the preview
3. Drag toward the Share note
4. Dropzone appears at cursor position
5. Drop inserts as a live QuoteBlock

### Smart Paste (⌘V)
Paste copied content from any note. Fosta resolves the source and creates a QuoteBlock with proper transclusion anchors.

Both methods coexist.

## QuoteBlocks (live transclusion)

A QuoteBlock is a note with `type: quote` that references a source note and specific blocks within it (`sourceNoteId`, `sourceBlocks`, `snapshot_text`). Full quote schema: [Data Model](../engineering/data-model.md#note-types).

Content resolves dynamically from SQLite at render time. If the source note is edited, the QuoteBlock reflects the change.

**Visual treatment:** an unpublished QuoteBlock shows a subtle live/pulsing indicator to signal it will update if the source changes. Once published and frozen, this is replaced with a static "Frozen" label — a clear visual distinction between "still tracking the source" and "permanently fixed."

## Publishing

On publish:
- All transclusions are frozen — `snapshot_text` becomes the permanent content
- `published: true` added to frontmatter
- Live updates stop

## Quick Look — excluded

Quick Look is not available in Share. The middle preview pane already serves this function — it is the preview surface. Applying Quick Look would be redundant.

## File navigator (left panel)

Single-click opens a note in the middle preview pane. Double-click does nothing. Identical behaviour to Develop's file navigator but opens to preview pane instead of editor.

**Untagged Inbox notes are hidden from the navigator's default list, but remain findable via its search** — the same browsable-vs-reachable rule applied in Develop's navigator and Track's "Add new item" search. See `docs/views/develop.md` for the full reasoning.

## Reference wireframe

<img src="/fosta/assets/wireframes/share.svg" alt="share wireframe" style="width:100%;border:1px solid #302825;border-radius:6px;margin:1rem 0">

*Reference wireframe — three-pane layout with file navigator, preview pane, and Share note. Drag-to-quote shown mid-gesture. Recreated from early hand sketches, June 2026 — reference only, not final UI.*

