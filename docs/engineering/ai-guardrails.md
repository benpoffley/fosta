---
title: "AI Guardrails"
parent: "Engineering"
nav_order: 4
layout: default
---

# AI Guardrails

**Status:** Final — Non-negotiable  
**Audience:** AI coding agents (Claude Code, Cursor, etc.)

These constraints exist to prevent architectural drift. Violating them is not a style issue — it breaks fundamental product principles. See the full ADRs in `decisions/` for the reasoning behind each constraint.

## Never do these things

| Constraint | Reason |
|---|---|
| Never suggest Electron | Tauri is final — ADR-001 |
| Never replace Markdown storage with a database or proprietary format | Markdown on disk is the source of truth — ADR-002 |
| Never use file paths as note identifiers | Always use UUIDs — ADR-005 |
| Never copy note content into canvases or Share documents | Always use UUID references — ADR-009 |
| Never call filesystem APIs directly | Always go through StorageAdapter — ADR-004 |
| Never use tldraw's internal format for on-disk storage | JSON Canvas only — ADR-006, ADR-011 |
| Never make core functionality network-dependent | Offline-first — ADR-007 |
| Never introduce a parallel object type | Everything is a note — ADR-008 |
| Never regenerate block UUIDs | Permanent once created — ADR-012 |
| Never suggest AI features | Out of scope for v1 |
| Never suggest real-time sync or collaboration | Out of scope for v1 |
| Never use plain JavaScript | Always TypeScript |
| Never use synchronous storage operations | Always async/await |

## Always do these things

| Constraint | Reason |
|---|---|
| Always explain what code does before generating it | Creator has no formal coding background |
| Always flag architectural implications | Especially around storage, data model, cross-view state |
| Always prefer readability and explicitness over clever abstractions | Maintainability matters |
| Always assume macOS as the primary target | v1 is macOS only |
| Always check ADRs before suggesting structural changes | Decisions are documented for a reason |
| Always use StorageAdapter for all note operations | Never bypass to filesystem |
| Always preserve block UUIDs through Tiptap serialisation | Comment/transclusion anchors depend on it |

## Scope discipline

If asked to implement something on the out-of-scope list, stop and flag it. These are deliberately excluded, not forgotten features.

**Out of scope for v1:** Google Drive sync · iPad app · Web app · AI features · Plugin system · Collaboration · Real-time sync · Bi-directional graph links · User accounts/auth · Tag templates · Tag hierarchy · Co-occurrence lines in Sort · Live transclusion across Cloudflare · Split view in Layer · Track (moves to v1.1 if not ready post-Sort)

## Naming — do not change

Three tab names are pending final decision. Use these placeholders:

| View | Placeholder | Candidates |
|---|---|---|
| First tab | Desk / Base | Desk, Base |
| Organisation view | Sort | Sort, Curate |
| Editor view | Layer | Layer, Work, Develop |

Canonical terms that must not be renamed: Quick Look · Global Actions · Add to Desk · Create Track from tag · QuoteBlock
