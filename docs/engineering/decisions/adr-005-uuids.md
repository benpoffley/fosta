---
title: "ADR-005: UUIDs"
parent: "Decision Records"
grand_parent: "Engineering"
nav_order: 5
layout: default
---

# ADR-005: UUIDs as stable note identity — never file paths

**Status:** Final  
**Date:** June 2026

## Decision
Every note has a UUID in YAML frontmatter (`id: <uuid>`). All references use UUID. File paths are incidental and may change.

## Context
Obsidian and similar tools reference notes by filename. When a file is renamed, all links break unless the tool updates them globally — fragile and fails silently in edge cases.

## Why UUIDs
- A UUID never changes regardless of filename or folder location
- Rename a note, move it — every reference survives automatically
- Required for the storage adapter pattern — non-filesystem backends have no file paths

## Wikilink syntax
`[[note-title|uuid]]` — display name is human-readable, UUID is the actual reference. SQLite indexes these for reverse lookup.

## Why not to revisit
Every reference in the system — wikilinks, canvas nodes, transclusions, block comments — depends on UUID stability. Switching to file paths would break every reference and re-introduce rename-fragility.
