---
title: "ADR-012: Block UUIDs"
parent: "Decision Records"
nav_order: 12
layout: default
---

# ADR-012: Block UUIDs are permanent anchors

**Status:** Final  
**Date:** June 2026

## Decision
Every Tiptap block receives a UUID at creation, stored as a node attribute and serialised as an HTML comment: `<!-- id: 550e8400 -->`. Block UUIDs are generated once and never regenerated.

## Why permanent
- Block comments anchor to `parentBlock: <uuid>` — stability required for them to stay attached to the correct block
- Transclusions anchor to block UUID + character offsets — not absolute positions, which shift as content is edited
- HTML comment serialisation keeps UUIDs inside the Markdown file — no external index required

## Critical build constraint
If block UUIDs are blown away on a save/reload cycle — which can happen if Tiptap serialisation doesn't explicitly preserve node attributes — all comment anchors and transclusion anchors silently break. This must be explicitly handled and tested in the serialisation layer.

## Why not to revisit
Every block-level comment and every transclusion depends on UUID stability. This is a hard requirement, not a preference.
