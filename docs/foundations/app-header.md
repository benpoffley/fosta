---
title: "App Header"
parent: "Foundations"
nav_order: 3
layout: default
---

# App Header

**Status:** Decided for most views — Desk's treatment is an open question  
**Type:** Foundations — global, cross-view

A persistent header bar at the top of the window, consistent across views.

## Structure, left to right

| Element | Position | Behaviour |
|---|---|---|
| Fosta logo | Far left | Brand mark, static |
| Vault switcher | Next to logo | Shows current vault name. Click opens the known-vaults dropdown, with "Add new vault…" inline. See [Settings](settings.md) for the quick-add-vs-manage split this follows. |
| Search | Centred | Global search — behaviour is view-dependent; on Sort it searches notes and tags simultaneously (see `docs/views/sort.md`) |
| Capture | Far right | Opens the Capture surface (Quick Look, note variant). See [Capture](capture.md). |

See also [View Switcher](view-switcher.md) — the separate persistent control for moving between views.

## Consistency across views

The header is the same on every view — same logo position, same vault switcher, same Capture button. This is what makes it a Foundation rather than a view-specific layout choice.

**Desk is the open exception.** Desk has its own persistent, always-visible capture bar built into the canvas itself (see `docs/views/desk.md` — Timer pill section). Whether Desk keeps that inline bar *in addition to* the header's Capture button, or the header replaces it entirely, is **not decided**. Until it is, treat Desk's existing capture bar documentation as valid, and do not assume the header silently overrides it.
