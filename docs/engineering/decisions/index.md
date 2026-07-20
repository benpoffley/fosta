---
title: "Decision Records"
parent: "Engineering"
nav_order: 6
has_children: true
layout: default
---

# Architectural Decision Records

Numbered records of every major architectural decision. Highest authority in this wiki. Do not reopen casually — if a decision must change, document it as a new ADR.

| ADR | Topic | Summary | Status |
|---|---|---|---|
| [ADR-001](adr-001-tauri.md) | Tauri 2 as desktop shell | Tauri wraps React/TS frontend; lightweight, native filesystem + SQLite access | Final |
| [ADR-002](adr-002-markdown-source-of-truth.md) | Markdown as source of truth | All notes stored as .md files with YAML frontmatter; SQLite is rebuildable index | Final |
| [ADR-003](adr-003-sqlite-index.md) | SQLite index layer | Fast querying via SQLite, always rebuildable from markdown files | Final |
| [ADR-004](adr-004-storage-adapter.md) | Storage adapter pattern | All storage via StorageAdapter interface; never call filesystem APIs directly | Final |
| [ADR-005](adr-005-uuids.md) | UUIDs as stable identity | Every note has a UUID; all references use UUID not file path | Final |
| [ADR-006](adr-006-json-canvas.md) | JSON Canvas for canvases | Canvas files stored as JSON Canvas; tldraw renders only | Final |
| [ADR-007](adr-007-offline-first.md) | Offline-first | Core functionality works without internet; Cloudflare is work-access only | Final |
| [ADR-008](adr-008-everything-is-a-note.md) | Everything is a note | Comments, quotes, share docs — all notes with different frontmatter | Final |
| [ADR-009](adr-009-references-not-copies.md) | References not copies | Notes in Track, Share, Desk are UUID references; content never duplicated | Final |
| [ADR-010](adr-010-cloudflare-work-capture.md) | Cloudflare work-access | Browser form → Cloudflare → local inbox; read-only vault snapshot in R2 | Final |
| [ADR-011](adr-011-tldraw-renderer-only.md) | tldraw as renderer only | tldraw renders JSON Canvas; never stores data in its own format | Final |
| [ADR-012](adr-012-block-uuids.md) | Block UUIDs | Permanent stable UUIDs for every Tiptap block | Final |
| [ADR-013](adr-013-platform-strategy.md) | Platform and mobile strategy | Mac-only v1, mobile deferred, full cloud-first trade-off analysis documented | Decided |
