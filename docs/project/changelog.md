---
title: "Changelog"
parent: "Project"
nav_order: 6
layout: default
---

# Changelog

Documentation milestones. Records meaningful changes — not every wording edit.

Where decisions evolve, preserve historical context. Do not delete previous decisions — document what changed, why, and when.

---

## v0.2.0 — July 2026

- Migrated wiki from single HTML file to multi-file Markdown repo
- Added CLAUDE.md as primary AI agent entry point
- Added build/ layer (BUILD-GUIDE.md, build-sequence.md, file-structure.md, acceptance-criteria.md)
- Added How to Use / order of authority documentation
- Added all 12 Architectural Decision Records (ADR-001 through ADR-012)
- Added AI Guardrails section
- Added Known Compromises section
- Added Current Development section
- Added Global Actions and Quick Look pages under Foundations
- Established Quick Look as canonical term (supersedes "expanded capture card")
- Documented canvas node two-state model (compact / expanded live preview)
- Settled interaction model: single click selects, double click opens Quick Look
- Track: Linear → Line, Spatial → Web; Thread added (vertical scroll, no canvas/connections/annotations, read-only v1)
- Track population model formalised: Manual vs Live (AND-only tag combination), Create-new pre-tagging, Live tracks skip Inbox
- File navigator in Track scoped to Manual tracks only
- Known Compromises: Web mode renamed from Spatial, Thread inline editing added as v1.1 candidate
- Future Ideas page added — Line/Thread zoom model exploration documented for post-v1
- Updated Desk, Layer, Capture docs to reflect new interaction model
- Restructured nav: Product / Market / Engineering / Foundations / Views / Project
- Sort prototype updated: shared top bar layout, floating search, vertical dividing line, tag orb visual treatment (no resting stroke), multi-tag filter (AND logic), persistent selection for multi-tag apply, Clear sorted button, tag creation (+ New tag button + search-to-create)
- Sort view page fully rewritten with all interaction decisions documented

## v0.1.0 — June 2026

- Initial wiki created — product brief, philosophy, architecture, data model, all views
- Competitive landscape research completed
- Smart Paste upgraded from snapshot to live transclusion — brought into v1 scope
- Track fully specified: Linear and Spatial modes, annotation anchoring model
- Share view updated from two-pane to three-pane layout
- Canvas-reference nodes added to Desk scratchpad
- Reference wireframes recreated from hand sketches
- Capture restructured as a Foundation, not a view
- Global Actions principle established; two v1 implementations documented
