# Fosta Build Guide

**For:** AI coding agents (Claude Code, Cursor) and future collaborators  
**Read first:** `../CLAUDE.md` — contains the hard constraints and guardrails

## Before writing any code

1. Read `CLAUDE.md` — the guardrails are non-negotiable
2. Read `../docs/engineering/architecture.md` — the nine principles
3. Read the relevant ADRs in `../docs/engineering/decisions/`
4. Check `../docs/project/current-development.md` — build the right thing right now
5. Read the view spec in `../docs/views/` for the feature you're implementing

## How this product is built

Fosta is built phase by phase. Each phase must be working and in daily use before the next begins. Do not skip ahead.

See `build-sequence.md` for the ordered task list.

## The three-layer mental model

```
UI Layer (React components)
    ↓ reads/writes via
StorageAdapter interface
    ↓ implemented by
LocalFilesAdapter (Markdown files + SQLite)
```

Never collapse these layers. Never let UI components call filesystem APIs. Always go through the adapter.

## Key implementation constraints

### Storage
- All note operations go through `StorageAdapter` — see `../docs/engineering/architecture.md`
- SQLite is a rebuildable index, never the source of truth
- Never hardcode file paths — use the adapter's list/get/save/delete methods

### Notes
- Every note needs a UUID in frontmatter at creation time
- Every note needs `id`, `created`, `modified`, `tags` in frontmatter
- Type field only added for non-standard notes: `comment`, `quote`, `share`

### Tiptap / Editor
- Every block must have a stable UUID as a node attribute
- Serialise block UUIDs as HTML comments: `<!-- id: uuid -->`
- Never regenerate block UUIDs on load — read from HTML comments
- Test serialisation round-trip explicitly: create note → save → reload → confirm UUIDs unchanged

### Canvas
- Store as JSON Canvas format — never tldraw's internal format
- Translate between JSON Canvas and tldraw at runtime via adapter
- Node types: `freeform`, `note-reference`, `canvas-reference`
- Node states: `compact`, `expanded`

### TypeScript
- Always TypeScript, never JavaScript
- Define types for Note, NoteMetadata, StorageAdapter, CanvasNode etc. before implementing
- Avoid `any` — if you need it, flag it as technical debt

### Async
- Every storage operation is async/await
- No synchronous filesystem calls anywhere

## Component naming conventions

Use these names consistently — do not invent alternatives:

| Concept | Component/file name |
|---|---|
| Quick Look modal | `QuickLook` |
| Note Quick Look | `QuickLookNote` |
| Canvas Quick Look | `QuickLookCanvas` |
| Bottom toolbar | `Toolbar` |
| Capture bar | `CaptureBar` |
| Tag frequency bubble | `TagBubble` |
| Note card (inbox) | `NoteCard` |
| Canvas node (compact) | `CanvasNodeCompact` |
| Canvas node (expanded) | `CanvasNodeExpanded` |
| File navigator | `FileNavigator` |
| Layers panel | `LayersPanel` |

## What "done" looks like

See `acceptance-criteria.md` for per-feature definitions of done.

General principle: a feature is done when it works correctly, handles errors gracefully, and the data it writes can be read back correctly after a restart. Fosta's data must be valid Markdown + YAML at all times.
