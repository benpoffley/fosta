---
title: "Architecture"
parent: "Engineering"
nav_order: 2
layout: default
---

# Architecture Principles

**Status:** Final — non-negotiable

Nine principles govern every engineering decision in Fosta. Each links to its full ADR with context, rationale, and why it should not be casually revisited.

| # | Principle | Summary | ADR |
|---|---|---|---|
| 1 | Storage adapter pattern | All storage through `StorageAdapter` — never call filesystem APIs directly | [ADR-004](decisions/adr-004-storage-adapter.md) |
| 2 | UUIDs as stable identity | Every note has a UUID; all references use UUID; file paths are incidental | [ADR-005](decisions/adr-005-uuids.md) |
| 3 | Everything is a note | Comments, quotes, share docs — all notes with different frontmatter; no parallel object types | [ADR-008](decisions/adr-008-everything-is-a-note.md) |
| 4 | References not copies | Notes in Track, Share, Desk are UUID references to originals; content never duplicated | [ADR-009](decisions/adr-009-references-not-copies.md) |
| 5 | Async everywhere | Every storage call is async/await; no exceptions | — |
| 6 | Offline-first | Core functionality works without internet; Cloudflare is work-access only | [ADR-007](decisions/adr-007-offline-first.md) |
| 7 | YAML frontmatter required | Every note needs `id`, `created`, `modified`, `tags`; OS file dates are unreliable | [ADR-002](decisions/adr-002-markdown-source-of-truth.md) |
| 8 | Block UUIDs permanent | Every Tiptap block gets a UUID at creation, serialised as HTML comment; never regenerated | [ADR-012](decisions/adr-012-block-uuids.md) |
| 9 | JSON Canvas for canvases | Canvas files stored as JSON Canvas; tldraw renders only, never stores | [ADR-006](decisions/adr-006-json-canvas.md), [ADR-011](decisions/adr-011-tldraw-renderer-only.md) |

## Data flow

```
Markdown files (source of truth)
       ↓ scan on startup / file watch
    SQLite index (rebuildable)
       ↓ queries
  React UI (Zustand state)
       ↓ saves via StorageAdapter
Markdown files (source of truth)
```

SQLite is always rebuildable from the Markdown files. If the index is lost or corrupted, re-scan the vault. Never treat SQLite as the source of truth.

## Platform and mobile strategy

The decision to build Mac-native with Tauri, and the reasoning for deferring mobile and cross-device support, is documented in [ADR-013](decisions/adr-013-platform-strategy.md). This includes the full trade-off analysis of local-first vs cloud-first, options for future mobile development, and the conditions under which the platform strategy should be revisited.
