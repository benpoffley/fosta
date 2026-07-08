---
title: "Scope"
parent: "Project"
nav_order: 3
layout: default
---

# Scope

## In scope for v1.0

- Capture (global action, Quick Look modal, Inbox)
- Desk / Base (scratchpad canvas, freeform + note-reference + canvas-reference nodes, timer pill)
- Sort (inbox grid, tag-frequency bubbles, Create Track from tag)
- Layer / Work / Develop (three-panel editor, Tiptap, block comments, wikilinks)
- Track (Linear + Spatial modes, note-reference nodes, annotations, connections)
- Share (three-panel, QuoteBlocks, drag-to-quote, Smart Paste, publish)
- Quick Look (note + canvas variants, wikilink back-stack)
- Global Actions (Add to Desk, Create Track from tag)
- Cloudflare work-capture (browser form → D1 → Mac inbox)
- Read-only vault snapshot (R2)
- LocalFilesAdapter (Markdown on disk + SQLite index)

## Out of scope for v1 — do not build or suggest

| Item | Notes |
|---|---|
| Google Drive sync | v2 via StorageAdapter |
| iCloud sync | v2 via StorageAdapter |
| iPad app | v2 in SwiftUI |
| Web app | v2+ |
| AI features of any kind | Including auto-backlinks, suggested tags |
| Plugin system | — |
| Collaboration / multi-user | — |
| Real-time sync | — |
| Bi-directional graph links | Manual wikilinks are the v1 model |
| User accounts / auth | — |
| Tag templates | Insufficient v1 value |
| Tag hierarchy / nesting | Flat tags only |
| Co-occurrence lines in Sort | Downgraded to maybe-later |
| Live transclusion across Cloudflare | snapshot_text only at work |
| Split view in Layer | v1.1 |
| Track (if not ready post-Sort) | Moves to v1.1 |
