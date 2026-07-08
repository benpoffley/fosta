# Product Philosophy

**Status:** Decided — Final

## Core principles

### All views are lenses on the same data
The user's mode of working determines which view they're in, not which data they can access. Sort, Layer, Track, and Share all operate on the same underlying vault. There is no "Sort data" vs "Layer data" — there is only one set of notes, seen differently.

### Everything is a note
Comments, quote blocks, share documents, Track annotations — all resolve to notes with different frontmatter metadata. No separate object types. One storage interface, one query model, one set of rules.

### References, not copies
Notes appearing in Track canvases, Share documents, or the Desk scratchpad are UUID references to originals. Content is never duplicated. If a source note is deleted, references surface a warning rather than silently breaking.

### Organise by what something is, not where it lives
Tags are flat, single-level arrays. No hierarchy. No folder-based type schemas. Organisation is emergent from tagging and linking, not enforced by structure.

### The pipeline is the product
The DaVinci Resolve page metaphor is not decorative — it is the product. Each view is a mode of working. Moving through the views is the workflow.

### Offline-first, local-first
The network is an enhancement, not a requirement. Core functionality works without internet. User data is plain text files on the user's own device.

## Global affordances

Certain actions belong to a UI element, not a view. They work wherever that element is rendered:

- **Capture bar** — available from any view
- **Create Track from tag** — hover any tag anywhere it renders
- **Add to Desk** — overflow menu on any note, direct button in Quick Look and Layer

The broader principle — "create or send any item from any context where it makes sense" — is a direction for future expansion. v1 implements these three.

## Design principles

- Splitting views into sub-screens for minor state differences is wrong
- The bottom toolbar is always visible — navigation is never more than one tap away
- Deferred complexity: features that add architectural weight without proportional user value belong in v2
- Build phase discipline: use each phase before proceeding to the next
