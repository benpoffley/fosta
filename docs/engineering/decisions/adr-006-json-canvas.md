---
title: "ADR-006: JSON Canvas"
parent: "Decision Records"
nav_order: 6
layout: default
---

# ADR-006: JSON Canvas as canvas storage format

**Status:** Final  
**Date:** June 2026

## Decision
Canvas files (Desk scratchpad, Track canvases) are stored as JSON Canvas format on disk. tldraw is the renderer only — its internal format is never written to disk.

## Context
Fosta uses tldraw as its canvas renderer. tldraw has its own internal data format. Storing tldraw's internal format would couple on-disk representation to a specific version of a third-party library.

## Why JSON Canvas
- Open specification (MIT licensed) — not tied to any vendor
- Human-readable JSON — consistent with the markdown-on-disk philosophy
- Already supported by Obsidian and other tools — canvas files are portable
- A thin adapter translates between JSON Canvas and tldraw's internal format at runtime

## Why not to revisit
tldraw's internal format is a third-party implementation detail. If it changes in a major version, all canvas files would break. JSON Canvas is stable, open, and portable.
