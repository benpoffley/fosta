---
title: "Known Compromises"
parent: "Engineering"
nav_order: 5
layout: default
---

# Known Compromises

**Status:** Final — intentional decisions  
**Purpose:** Prevent future AI assistants from "fixing" deliberate design choices.

These are not bugs. They are deliberate decisions that accept a known limitation in exchange for reduced complexity, faster delivery, or better v1 scope alignment.

## Architecture compromises

| Compromise | Why it exists | Future path |
|---|---|---|
| Work capture uses polling, not real-time push | Real-time push requires persistent WebSocket and more complex infrastructure. Polling solves the problem adequately for v1. | v2 sync engine |
| No editing at work — read-only vault | Bidirectional sync requires conflict resolution. One-way flows eliminate this entirely. | v2 sync engine |
| Live transclusion doesn't work across Cloudflare | Work vault is a static snapshot. Live resolution requires the local SQLite index. | v1.1+ |
| macOS only | Focus. Building one platform well beats many platforms adequately. | iPad v2; web v2+ |

## Feature compromises

| Compromise | Why it exists | Future path |
|---|---|---|
| Backlinks are manual only | Auto backlink indexing requires continuous background scanning. Manual wikilinks give explicit control. | AI auto-backlinks v2+ |
| Tags are flat — no hierarchy | Tag hierarchy adds schema, UI, and query complexity without proportional value for v1. | Evaluate post-v1 |
| Track connections are visual-only | Relationship data structures are out of scope. Visual-only connections allow spatial canvas without graph infrastructure. | Not planned |
| Track Spatial mode: manual population only | Auto-layout algorithm for node placement is out of scope for v1. | v2 consideration |
| Sort inbox interaction model not finalised | Drag-to-tag model still being designed. Deferring prevents building interactions that need rebuilding. | Sort hi-fi design phase |
| Split view in Layer deferred | Multiple editor instances with independent state adds significant React complexity. | v1.1 |
| No tag templates | Insufficient value to justify v1 scope. | Evaluate post-v1 |

## Data model compromises

| Compromise | Why it exists |
|---|---|
| Block UUIDs serialised as HTML comments | Cleanest approach for block-level metadata within Markdown that doesn't break human-readability. Not pretty, but lossless and parseable. |
| Wikilinks use `display-name\|uuid` syntax | Pure UUIDs are unreadable in raw Markdown. Display-name-only links break on rename. Combined syntax is a deliberate compromise. |
