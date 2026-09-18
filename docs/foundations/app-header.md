---
title: "App Header"
parent: "Foundations"
nav_order: 3
layout: default
---

# App Header

**Status:** Decided for most views — Desk's treatment is an open question  
**Type:** Foundations — global, cross-view

A persistent header bar at the top of the window, consistent across views. Confirmed during hi-fi prototyping on Sort — this supersedes the earlier model where Capture lived in the bottom toolbar and the vault switcher was a standalone top-left control.

## Structure, left to right

| Element | Position | Behaviour |
|---|---|---|
| Fosta logo | Far left | Brand mark, static |
| Vault switcher | Next to logo | Shows current vault name. Click opens the known-vaults dropdown, with "Add new vault…" inline. See [Settings](settings.md) for the quick-add-vs-manage split this follows. |
| Search | Centred | Global search — behaviour is view-dependent; on Sort it searches notes and tags simultaneously (see `docs/views/sort.md`) |
| Capture | Far right | Opens the Capture surface (Quick Look, note variant). See [Capture](capture.md). |

## What this changes

**Capture moves out of the bottom toolbar.** It was previously a persistent + button in the toolbar (transient on most views, always-visible on Desk). It is now a single, consistently-placed header button — one location, not a view-dependent one. See [Capture](capture.md) for the updated behaviour.

**The vault switcher moves into the header.** It was previously documented as its own standalone control in the top-left of the window. It now sits inside the header, immediately right of the logo. The interaction model (dropdown, quick-add inline, manage in Settings) is unchanged — only its position and visual context changed, from a freestanding control to a header element.

**The bottom toolbar is now navigation-only.** With Capture removed, the toolbar holds only the five view tabs. It has also changed shape — see [Global Actions](global-actions.md) is unaffected, but the toolbar's own visual form is now a **floating, detached pill** rather than a bar fixed flush to the bottom edge. This is a visual/layout change confirmed during hi-fi prototyping, not yet formally documented as its own decision beyond this note — see `docs/views/sort.md` for the prototype that established it.

## Consistency across views

The header is intended to be **the same header on every view** — same logo position, same vault switcher, same Capture button. This is what makes it a Foundation rather than a Sort-specific layout choice.

**Desk is the open exception.** Desk currently has its own persistent, always-visible capture bar built into the canvas itself (see `docs/views/desk.md` — Timer pill section, "Persistent capture bar at centre-bottom, above the toolbar"). Whether Desk keeps that inline bar *in addition to* the global header's Capture button, or whether the global header replaces it entirely, has **not been decided** — it needs its own discovery pass rather than being resolved by extension from Sort. Until that discovery happens, treat Desk's existing capture bar documentation as still valid, and do not assume the global header silently overrides it.

## Where this was decided

Confirmed through hi-fi prototyping on the Sort view (two work-in-progress visual explorations — dark and light — both include this header). See `docs/views/sort.md` and `docs/project/story.md` for the fuller narrative.
