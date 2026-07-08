---
title: "ADR-009: References not copies"
parent: "Decision Records"
grand_parent: "Engineering"
nav_order: 9
layout: default
---

# ADR-009: References not copies

**Status:** Final  
**Date:** June 2026

## Decision
Notes in Track canvases, Share documents, or the Desk scratchpad are UUID references to originals. Content is never duplicated. Deleted source → warning, not silent break.

## Why references
- Single source of truth — editing a note updates its representation everywhere
- No divergence — no "which version is real?" problem
- Live transclusion in Share is only possible because QuoteBlocks reference originals by UUID

## Why not to revisit
The entire transclusion system, Share's QuoteBlocks, and the canvas node model depend on this. Introducing content duplication would create a maintenance and consistency problem throughout the codebase.
