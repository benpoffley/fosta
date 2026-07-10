---
title: "Future Ideas"
parent: "Project"
nav_order: 9
layout: default
---

# Future Ideas

A holding space for ideas that were explored, found to have real merit, but consciously deferred past v1. These are not "maybe someday" notes — they are documented because real thinking went into them and that thinking should not be lost.

The goal is that when v1.1 or v2 planning begins, these pages give enough context to pick up where the conversation left off without re-litigating decisions already made.

---

## Line + Thread as a single zoom-based view

**Status:** Deferred to post-v1. Currently implemented as two separate modes.
**Relevant current docs:** `docs/views/track.md`

### The idea

Rather than Line and Thread being two separate layout modes chosen at creation, they could be the same view at different zoom levels:

- **Zoomed out** — the sequence axis with compact node cards, connections, and annotations. The full canvas experience. Identical to current Line.
- **Zoomed in** — nodes expand to full note content. Connections and annotations recede. Feels like current Thread.

Orientation (horizontal vs vertical axis) would be a simple toggle, not a mode choice at creation.

The result: one fewer concept. Instead of explaining Line vs Thread, you just have Line — a sequence you can view at different scales. The zoom metaphor maps naturally to Fosta's video editing DNA (DaVinci Resolve's timeline zoom is a direct analogue).

### Why it was deferred

Two reasons:

**1. The "I just want Thread" user exists.** Some users want a pure reading experience — full note content, vertical scroll, no canvas semantics. With the zoom model, they'd have to create a Line track, set it to vertical, and zoom in. More steps, more concepts to understand before getting to the thing they want.

**2. It's a more ambitious UI design problem.** Line with the zoom model becomes a view with orientation, zoom level, and population mode as simultaneous variables. More powerful, but more cognitive load per view even if there are fewer views total.

The simpler path for v1 is two separate modes that are each easy to understand. Once users have been observed actually using Line and Thread, there will be real evidence about whether they feel like different activities at different scales (zoom model is right) or genuinely different modes of working (separate views are right).

### What to investigate post-v1

- Do users naturally switch between Line and Thread for the same set of notes, or do they create one or the other at the start and stay in it?
- Would the zoom threshold feel natural, or would users find it hard to control?
- How do connections and annotations behave at intermediate zoom levels — do they fade, collapse, hide?
- How do annotations appear when zoomed in to full-content view? Could they appear inline below the note, like margin notes? That could actually be a better UX than the current anchored-to-node model.
- What does the orientation toggle look like — is it a button, a gesture, a setting at creation that can be changed later?

### Relationship to current architecture

The current architecture does not preclude this. Thread is stored as JSON Canvas on disk (same format as Line), with tldraw not invoked for rendering. If the zoom model is adopted, the distinction becomes: below a zoom threshold, render via tldraw (Line experience); above a threshold, render as a scroll list (Thread experience). The on-disk format already supports both interpretations.

---

## Additional ideas

*This page will grow as more post-v1 ideas are explored and deferred. Add new sections here rather than losing them to conversation history.*

