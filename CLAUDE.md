---
title: "AI Agent Instructions"
nav_order: 2
layout: default
---

# Fosta — AI Coding Agent Instructions

Read this file before writing any code, suggesting any architecture, or proposing any feature change. This is the primary context document for AI coding tools working on Fosta.

## What Fosta is

A macOS-native, offline-first note-taking and idea-development app. Notes flow through five views (Desk, Sort, Layer, Track, Share) switchable via a persistent bottom toolbar. Capture is a global action available from any view.

The thesis: *capture fast, sort later, develop in layers, sequence, publish.*

## Stack — final, do not suggest alternatives

| Layer | Choice |
|---|---|
| Desktop shell | Tauri 2 |
| UI | React 18 + TypeScript |
| Styling | Tailwind CSS |
| Editor | Tiptap |
| Canvas renderer | tldraw (renderer only — never storage) |
| Storage: source of truth | Markdown files on disk + YAML frontmatter |
| Storage: index/query | SQLite via Tauri SQL plugin |
| State | Zustand |
| Backend (work access only) | Cloudflare Workers + R2 + D1 |

## Hard constraints — never violate these

```
NEVER suggest Electron — Tauri is final (ADR-001)
NEVER replace Markdown storage with a database or proprietary format (ADR-002)
NEVER use file paths as note identifiers — always UUIDs (ADR-005)
NEVER copy note content into canvases or Share — always UUID references (ADR-009)
NEVER call filesystem APIs directly — always StorageAdapter interface (ADR-004)
NEVER write tldraw's internal format to disk — JSON Canvas only (ADR-006, ADR-011)
NEVER make core features network-dependent — offline-first (ADR-007)
NEVER introduce a parallel object type — everything is a note (ADR-008)
NEVER regenerate block UUIDs — permanent once created (ADR-012)
NEVER suggest AI features — out of scope for v1
NEVER suggest real-time sync or collaboration — out of scope for v1
NEVER use plain JavaScript — always TypeScript
NEVER use synchronous storage operations — always async/await
```

## Always do these

```
ALWAYS explain what code does before generating it
ALWAYS flag architectural implications before implementing
ALWAYS prefer explicit over clever
ALWAYS use the StorageAdapter interface for note read/write
ALWAYS preserve block UUIDs through the Tiptap serialisation cycle
ALWAYS assume macOS as the primary target
ALWAYS check ADRs before suggesting structural changes
```

## Key architectural patterns

Full schemas and code examples are in `docs/engineering/data-model.md`. Summaries:

- **StorageAdapter** — all storage through this interface, never Tauri filesystem APIs directly. Methods: `listNotes()`, `getNote(id)`, `saveNote(id, content)`, `deleteNote(id)`, `watchForChanges(callback)`.
- **Note frontmatter** — every note requires `id` (UUID), `created`, `modified`, `tags` in YAML frontmatter.
- **Block UUIDs** — every Tiptap block has a stable UUID serialised as an HTML comment `<!-- id: uuid -->`. Never regenerate.
- **Wikilinks** — `[[display-name|uuid]]` syntax. UUID is the reference, display name is human-readable.
- **Note types** — expressed via `type` frontmatter: `comment`, `quote`, `share`. Standard notes have no type field.

## Where to find things

| What you need | Where it is |
|---|---|
| Full architectural decisions | `docs/engineering/decisions/` |
| Complete view specs | `docs/views/` |
| Capture spec | `docs/foundations/capture.md` |
| Canvas node model + interaction patterns | `docs/foundations/canvas-nodes.md` |
| Quick Look modal | `docs/foundations/quick-look.md` |
| Global Actions | `docs/foundations/global-actions.md` |
| Data model + schemas | `docs/engineering/data-model.md` |
| Build sequence | `build/build-sequence.md` |
| Target file structure | `build/file-structure.md` |
| Out of scope list | `docs/project/scope.md` |
| Known compromises | `docs/engineering/known-compromises.md` |

## Three tab names are pending — use these placeholders

| View | Placeholder | Candidates |
|---|---|---|
| First tab | Desk / Base | Desk, Base |
| Organisation view | Sort | Sort, Curate |
| Editor view | Layer | Layer, Work, Develop |

## Out of scope for v1 — do not implement or suggest

Google Drive sync · iPad app · Web app · AI features · Plugin system · Collaboration · Real-time sync · Bi-directional graph links · User accounts/auth · Tag templates · Tag hierarchy · Co-occurrence lines in Sort · Live transclusion across Cloudflare · Split view in Layer · Track (if not ready post-Sort, moves to v1.1)
