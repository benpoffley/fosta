---
title: "Current Development"
parent: "Project"
nav_order: 1
layout: default
---

# Current Development

**Status:** Updated regularly — lowest authority section  
**Current phase:** Pre-code — design & strategy

## Status overview

| Area | Status |
|---|---|
| Product brief | ✅ Complete |
| Lo-fi wireframes (all views) | ✅ Complete |
| Competitive research | ✅ Complete |
| Architecture decisions (ADRs 001–012) | ✅ Complete |
| Data model design | ✅ Complete |
| Product wiki (this repo) | ✅ Complete |
| Hi-fi Sort view (Figma) | 🔄 In progress |
| Sort interaction model (prototype) | ✅ Complete — see Sort page |
| Sort view design (all interactions) | ✅ Complete — ready for hi-fi Figma |
| Three view name decisions | ⏳ Deferred to hi-fi phase |
| Dev environment setup (M0) | ⏳ Not started |
| First GitHub commit | ✅ Complete — wiki repo initialised July 2026 |

## Current priority

**Hi-fi Sort view in Figma.** Sort locks the visual language for the entire app — typography, colour system, spacing, component patterns. Everything else follows from it. Do not start building before Sort hi-fi is resolved.

## Three naming decisions pending

Deferred to the hi-fi Figma phase. Will resolve when real screens and real copy make the right answer obvious. Candidates and status: [Open Questions](open-questions.md).

## Figma → specs process

Sort hi-fi is in progress in Figma. Before any coding begins, visual decisions made during hi-fi must be recorded back in the wiki:

- Colour values, typography sizes, and spacing confirmed in Figma → `docs/engineering/design-tokens.md`
- Interaction or layout changes that diverge from the lo-fi spec → relevant view spec (e.g. `docs/views/sort.md`)

This is a required process step, not optional cleanup. Code must reflect the documented spec, not a Figma file that has not been reconciled.

## Known blockers

- Three view names deferred

## Build sequence (high level)

See `../../build/build-sequence.md` for the full milestone build plan. Milestones are sequential and dependency-gated, not calendar-scheduled — each begins when the prior one is complete and in daily use.

| Milestone | Focus |
|---|---|
| M0 · Dev Environment | Tauri + React scaffold, tooling, first app commit |
| M1 · Data Foundation | Storage adapter, vault scanner, SQLite index |
| M2 · Capture | Capture bar, Quick Look, note intake |
| M3 · Sort | Inbox grid, tag bubbles, tagging model |
| M4 · Cloudflare Work Capture | Browser capture, D1 staging, R2 vault snapshot |
| M5–M6 · Develop → Share | Tiptap editor, block UUIDs, transclusion, publishing |
| M7 · Track | Line / Web / Thread canvases (provisionally v1) |
| M8 · Desk | Full scratchpad canvas, timer, history mode |
| M9 · Polish + Launch | Error states, performance, onboarding, payments, ship |
