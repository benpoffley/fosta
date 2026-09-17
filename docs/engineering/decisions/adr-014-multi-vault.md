---
title: "ADR-014: Multi-vault support"
parent: "Decision Records"
grand_parent: "Engineering"
nav_order: 14
layout: default
---

# ADR-014: Multi-vault support

**Status:** Decided for v1  
**Date:** August 2026

## Decision

Fosta supports multiple independent vaults, switchable via a global selector. A vault is a folder on disk containing its own notes, its own SQLite index, and its own view state — fully sealed from every other vault. `StorageAdapter` takes the vault's root path as runtime configuration, not a fixed value at build or launch time. Cloudflare work-access (ADR-010) is scoped to exactly one designated vault for v1.

## Context

The question that triggered this ADR: if a user already chooses a folder when setting up a vault, and everything is saved inside that folder anyway, what additional work does switching between folders actually require?

The honest answer is: less than it first appears, provided `StorageAdapter` is built with a swappable root path from the start — which costs nothing extra to build correctly now, but is expensive to retrofit later if `LocalFilesAdapter` is written assuming a single fixed root.

## Why this is valuable

A freelance creative professional working across multiple clients has a genuine need to keep contexts separate — not just visually, but structurally. A Sort tag cloud mixing a client's confidential project tags with personal ideas is actively harmful, not just untidy. Separate vaults solve this cleanly: each is a fully independent world with no risk of cross-contamination.

It also gives a clean portability property: pointing a vault at a specific folder means that folder is a complete, self-contained Fosta world, shareable or movable as a unit.

## What a vault actually is

A folder on disk containing:
- The user's notes as markdown files, exactly as today
- A hidden `.fosta/` folder holding that vault's own SQLite index (`.fosta/index.sqlite`) and its own persisted view state (which notes are open in Layer, which Track tabs are open and their zoom/pan, Desk's canvas state — see [View State Persistence](../../foundations/view-state-persistence.md))

Switching vaults means: tear down the current `StorageAdapter` instance, instantiate a new one pointed at the new root path, and reset all view state to whatever that vault's own `.fosta/` folder has recorded (or a fresh default, if the vault is new). This mirrors the existing principle that Sort always resets to Inbox — switching vaults is the same idea applied at the vault level: everything reloads to that vault's own last-known state, nothing carries over from the previous vault.

## What does not carry over between vaults

- **References** — a note UUID only has meaning within the vault that contains it. No cross-vault linking. This matches how established local-first tools with a similar vault concept behave.
- **View state** — see above. Each vault remembers its own state independently.
- **SQLite index** — each vault has its own; there is no shared or global index.

## Cloudflare work-access — scoped to one vault for v1

ADR-010 assumes exactly one vault syncing a read-only snapshot to R2 for remote capture. Multi-vault raises a real choice here: sync every vault independently, let the user designate one vault as cloud-enabled, or scope work-access to a single vault entirely.

**Decision for v1: work-access is scoped to exactly one designated vault.** The user selects which vault has work-access enabled; other vaults do not sync to Cloudflare at all. This avoids multiplying Cloudflare costs and sync complexity before there's any evidence multiple users need simultaneous remote capture into more than one vault.

**Left open for v2 or later:** allowing more than one vault to sync independently, or letting the user choose which vault a captured note should land in at the point of capture (e.g. a work-capture form that lets the user pick "Work" vs "Personal" before submitting). Nothing in this decision forecloses either — the D1/R2 architecture in ADR-010 does not need to change to add this later, only the routing logic that currently assumes a single destination vault.

## Where vault switching and management live in the UI

The vault switcher is a persistent, always-visible control — separate from the app's Settings surface, since switching vaults is a frequent action and Settings is reserved for rare, global configuration. Adding a new vault is exposed directly in the switcher (quick-add, low friction, since adding is always safe); renaming or removing a vault from the known-vaults list is Settings-only. See [Settings](../../foundations/settings.md) for the full mechanic and reasoning.

## What this requires, concretely

- `StorageAdapter` and `LocalFilesAdapter` (ADR-004): root path passed in as configuration at instantiation, not read from a fixed constant
- SQLite index (ADR-003): lives inside each vault's `.fosta/` folder, not in a single global location
- View state persistence: scoped per vault, stored in that vault's `.fosta/` folder
- A global vault switcher: lives above the five-view toolbar, not inside any single view — infrequent and global, unlike the deliberately chrome-free content areas within each view
- First-run and vault-management UI: folder picker, a list of known vaults, ability to remove a vault from that list (never deletes the folder itself, only forgets it), and graceful handling if a vault's folder has been moved or deleted on disk since it was last opened

## Why not to revisit

Building `StorageAdapter` with a fixed root path "for simplicity now" would need to be undone the moment multi-vault is added, touching every call site that currently assumes a single implicit vault. Building it with a swappable root from the start costs nothing extra today and avoids that rework entirely.
