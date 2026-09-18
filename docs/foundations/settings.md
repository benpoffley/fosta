---
title: "Settings"
parent: "Foundations"
nav_order: 6
layout: default
---

# Settings

**Status:** Decided — Final

Fosta's global, infrequent configuration surface. Settings is deliberately small — it holds only things that are genuinely app-wide and rarely touched, not view-specific controls that already have a natural home elsewhere.

## Access

Standard macOS convention: `Fosta → Settings…` menu, with the `⌘,` keyboard shortcut. No icon or entry point in the header or the [View Switcher](view-switcher.md) — Settings doesn't belong in that workflow loop.

This deliberately splits two things that might otherwise be bundled together:

- **Vault switching** — something a user might do multiple times a day, bouncing between contexts (work, personal, a specific client). This needs to be fast and always visible.
- **Settings** — something touched rarely. Burying a frequent action one level inside a rare-action menu would be backwards.

## Vault switcher — separate from Settings, always visible

The vault switcher is a persistent control, showing the name of the currently open vault. Clicking it opens a dropdown of known vaults. It lives inside the [App Header](app-header.md), immediately to the right of the Fosta logo.

### Quick-add vs. manage — the general pattern

Fosta draws a consistent line between two kinds of actions on any user-managed list (vaults, Desk wipe frequencies — see below): **adding** a new item is safe and additive, so it's exposed at the point of use with minimal friction. **Editing, renaming, or deleting** an existing item can affect something already depended on, so those actions are deliberately tucked into Settings, one step removed.

Applied to vaults:

- **"Add new vault…"** appears directly inside the vault switcher dropdown. Selecting it opens the folder picker; once a folder is chosen, Fosta switches to that vault immediately. No trip to Settings required.
- **Renaming a vault, removing it from the known-vaults list, or changing which vault has work-access enabled** — all Settings-only, under the Vaults category below.

Removing a vault from the list never deletes or touches its folder on disk — it only forgets the vault, the same way removing a bookmark doesn't delete the bookmarked page. See ADR-014 for the underlying vault architecture.

## What lives in Settings

### Vaults
- List of known vaults (name, folder path)
- Rename a vault
- Remove a vault from the list (does not delete the folder)
- Which vault has work-access (Cloudflare) enabled — see ADR-010 and ADR-014. Work-access is scoped to exactly one vault for v1.

### Work-access
- On/off toggle for Cloudflare work-access
- Device-pairing status. **No user account or login exists in Fosta v1** — there is no sync between machines, and work-access is a device-level pairing, not an identity system. Settings never implies a sign-in/sign-out concept, because none exists.

### Desk — wipe frequency options (the list, not the current selection)
The Desk Timer pill's frequency dropdown reads from a shared, editable list of options. Settings owns that list:
- A fixed set of default frequencies (e.g. 15 min / 1 hour / 4 hours / 24 hours) — **cannot be deleted**, ensuring the list is never empty
- Any custom frequencies the user has added — **can be edited or deleted here**

**Quick-add vs. manage, applied to wipe frequencies:** adding a new custom frequency can be done directly from the Timer pill's dropdown on Desk itself (an "Add custom frequency…" option within that same dropdown) — no trip to Settings needed, and the new value is immediately available and selected. Editing or deleting an existing custom frequency is Settings-only. This is the same pattern as vaults, applied to a different list.

Everything else about the wipe cycle — actually selecting a frequency for the current canvas, Wipe Now, History — remains entirely inside the Timer pill on Desk itself. There is no wipe-confirmation prompt, by design; all of that behaviour is documented in `docs/views/desk.md` and Settings has no other Desk-specific content.

### Tag management lives in Sort, not Settings

Tags are the one user-managed list that does **not** follow the quick-add/manage split above. Renaming a tag, and deleting a tag with no notes attached, both happen directly in [Sort](../views/sort.md#tag-management--rename-and-delete-directly-in-sort) — there is no Tags category here. Tags are worked with continuously as part of Sort's own core activity, not an occasional app-wide configuration change, so their management stays where that work already happens rather than being pulled into Settings the way vaults and wipe frequencies are.

An in-use tag cannot be deleted from Sort or from Settings — there is no bulk "remove this tag from all its notes" action in v1. Retiring an in-use tag means removing it from its remaining notes first.

### Sort
- Default sort order (date / title)
- Default view (grid / list)

### About
- App version
- Link to the wiki/documentation

## What is deliberately not here (yet)

- **Appearance / theming** — the botanical dark visual identity is currently the whole of Fosta's design language, not a user-togglable preference. A setting implies an alternative exists; none does yet, so nothing is added here speculatively.
- **Accounts** — no login, no sign-out, no user identity system. Revisit only if a future version introduces cross-device sync (see ADR-013), at which point Settings would need a real Account category — not before.
