# ADR-003: SQLite as the query and index layer

**Status:** Final  
**Date:** June 2026

## Decision
SQLite (via the Tauri SQL plugin) serves as an index layer over the markdown files. It is always rebuildable. The application never treats SQLite as the source of truth.

## Context
Markdown files cannot be efficiently queried. Finding all notes with a specific tag, or all notes referencing a UUID, requires scanning every file — too slow for real-time UI at scale.

## Why SQLite
- Fast structured queries — tag frequency, backlink lookup, transclusion resolution in milliseconds
- Available via first-party Tauri plugin — no additional native dependencies
- Widely understood by any developer or AI agent
- Rebuildable from Markdown if lost or corrupted

## Why not to revisit
SQLite is the right tool for this job. The constraint that it is always rebuildable and never the source of truth must be preserved.
