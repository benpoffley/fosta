---
title: "ADR-011: tldraw renderer only"
parent: "Decision Records"
grand_parent: "Engineering"
nav_order: 11
layout: default
---

# ADR-011: tldraw as renderer only — never as storage

**Status:** Final  
**Date:** June 2026

## Decision
tldraw renders canvases at runtime. A thin adapter translates between JSON Canvas (on-disk format) and tldraw's internal format. tldraw's internal representation is never written to disk.

## Why this separation
- If tldraw is upgraded or replaced, canvas files remain readable — only the adapter needs updating
- JSON Canvas is an open standard; tldraw's format is not
- Consistent with the principle of keeping storage formats open and portable

## Why not to revisit
The adapter is a small, well-defined layer. The benefit — permanent storage independence from a third-party library — vastly outweighs its maintenance cost.
