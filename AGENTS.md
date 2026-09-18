---
title: "AI Agent Instructions"
nav_order: 2
layout: default
---

# Fosta — AI Coding Agent Instructions

Read this file before writing any code, suggesting any architecture, or proposing any feature change. This is the primary context document for AI coding tools working on Fosta.

## Fetching this wiki — important for any AI tool

If you are an AI tool being pointed at this wiki via URL (Claude Design, another chat, a design tool, etc.), **do not fetch pages from `benpoffley.github.io/fosta/...`** — that domain is served through a CDN (Fastly, via GitHub Pages) which can return stale cached content to automated fetchers even when the live site is current in a browser.

**Always fetch the raw file instead:**
```
https://raw.githubusercontent.com/benpoffley/fosta/main/[path-to-file]
```
For example, to read the Track view spec:
```
https://raw.githubusercontent.com/benpoffley/fosta/main/docs/views/track.md
```
This serves the file directly from the repository with no CDN caching layer, guaranteeing you always see the current content. Use the `.md` file path exactly as it appears in the repo (not the rendered `.html` path used on the Pages site).

## What Fosta is

A macOS-native, offline-first note-taking and idea-development app. Notes flow through five views (Desk, Sort, Layer, Track, Share) switchable via a persistent bottom toolbar. Capture is a global action available from any view.

The thesis: *capture fast, sort later, develop in layers, sequence, publish.*

## Order of authority

When sections conflict, higher beats lower:

1. Architectural Decision Records (ADRs) — `docs/engineering/decisions/`
2. AI Guardrails — `AGENTS.md` and `docs/engineering/ai-guardrails.md`
3. Product philosophy + architecture — `docs/product/` and `docs/engineering/`
4. Feature specifications — `docs/views/` and `docs/foundations/`
5. Current development — `docs/project/current-development.md`
6. Roadmap — lowest authority

## Keep process out of product docs

`docs/product/`, `docs/views/`, and `docs/foundations/` describe **current state only**. Someone opening any of these pages should be able to read a clean, standalone spec without needing to know how Fosta got there.

**Never write into these pages:**
- "Confirmed during hi-fi prototyping…"
- "This replaced an earlier direction where…"
- "Was found to be disorienting…" / "…confusing" / any rationale framed as a discovery
- "As earlier documented…" / "Previously, this was…"
- Any comparison to a prior version, rejected alternative, or how a decision was reached

That narrative belongs in `docs/project/story.md` (the "why and how," full prototyping/decision history) and `docs/project/changelog.md` (terse "what changed and when") — **not duplicated into the spec it describes.** Write it once, in the right file. If a spec page needs updating because of a new decision, update the spec to state the new current truth plainly, and put the story of how you got there in `story.md` only.

**Exceptions — these pages exist specifically to track history/status, so process language belongs there:**
- `docs/project/open-questions.md` — tracking resolution of open items
- `docs/project/changelog.md` — a log, by definition
- `docs/project/story.md` — the narrative record, by definition
- `docs/engineering/design-tokens.md` — explicitly a WIP staging doc pending finalisation

**A quick self-check before writing to any `product/`, `views/`, or `foundations/` page:** if a sentence would still make sense to someone who has never seen a previous version of this doc, keep it. If it only makes sense by reference to what the doc used to say, cut it or move it to `story.md`.

## Stack — final, do not suggest alternatives

Tauri 2 · React 18 + TypeScript · Tailwind · Tiptap · tldraw (renderer only, never storage) · Markdown-on-disk source of truth (YAML frontmatter) · SQLite index (via Tauri SQL plugin) · Zustand · Cloudflare (work-access only).

Full stack table with rationale: `docs/engineering/stack.md`.

## Hard constraints — never violate these

The canonical, ADR-cited version of these constraints lives in `docs/engineering/ai-guardrails.md` — consult it for the authoritative per-ADR detail. The list below is a compact fast reference; if the two ever diverge, ai-guardrails.md wins.

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

## Three tab names are pending

Three view names are unresolved (first tab, organisation view, editor view). Use the current placeholders — Desk, Sort, Layer — until decided. Candidates and status: `docs/project/open-questions.md`.

## Out of scope for v1 — do not implement or suggest

Do not implement or suggest AI features, real-time sync / collaboration, or a plugin system, among others. Full list: `docs/project/scope.md`.
