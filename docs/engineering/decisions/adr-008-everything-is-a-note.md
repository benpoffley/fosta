---
title: "ADR-008: Everything is a note"
parent: "Decision Records"
nav_order: 8
layout: default
---

# ADR-008: Everything is a note — single object type

**Status:** Final  
**Date:** June 2026

## Decision
Every piece of content resolves to a single note object. Comments, quotes, share documents — all are notes with different frontmatter metadata. No separate Comment, Block, or Card type.

## Why one type
- One storage interface, one query model, one set of principles
- New "types" are frontmatter fields, not new database tables
- Philosophically coherent with "all views show the same data through different lenses"
- Future-proof — new content types don't require schema migrations

## Types in practice
`type: comment` · `type: quote` · `type: share` — standard notes have no type field.

## Why not to revisit
Introducing a parallel object type would undermine the foundational data architecture. The `type` frontmatter approach handles all current and foreseeable cases without this cost.
