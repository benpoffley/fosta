---
title: "ADR-002: Markdown source of truth"
parent: "Decision Records"
nav_order: 2
layout: default
---

# ADR-002: Markdown files on disk as the source of truth

**Status:** Final  
**Date:** June 2026

## Decision
Every note is a `.md` file on disk with YAML frontmatter. Markdown files are canonical. All other storage (SQLite) is derived and always rebuildable.

## Context
Note-taking apps that use proprietary formats trap user data. Format lock-in is both an ethical problem and a competitive weakness.

## Why Markdown
- Human-readable without any application — data is always accessible
- Portable — openable in any text editor, syncable with any service, git-versionable
- Future-proof — if Fosta ceases to exist, the user's notes remain intact and usable
- Core differentiator: "your data is plain text files on your own device"

## Alternatives considered
- **SQLite as source of truth:** Rejected. Opaque binary, creates data lock-in.
- **Proprietary JSON:** Rejected. Same lock-in without SQLite's query power.
- **Cloud database:** Rejected for v1. Contradicts offline-first. See ADR-007.

## Why not to revisit
The open/portable data story is a core product promise. SQLite is a derived index — if corrupted, it is rebuilt by re-scanning markdown files. Reversing this undermines a fundamental product principle.
