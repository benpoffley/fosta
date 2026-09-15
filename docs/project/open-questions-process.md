---
title: "Open Questions — Process"
parent: "Project"
nav_order: 5
layout: default
---

# Open Questions — Process

How to use `open-questions.md` and what belongs there vs elsewhere.

---

## What belongs where

| Item type | Where it goes |
|---|---|
| Genuinely unresolved question that blocks or shapes a decision | `open-questions.md` |
| Question that is deferred but not actively blocking | Note in the relevant view spec or `current-development.md` |
| Significant product decision that has already been made | `docs/project/story.md` (narrative context) |
| Architectural or technical decision that has been made | New ADR in `docs/engineering/decisions/` |
| Implementation detail or minor choice | Inline in the relevant spec — no separate tracking needed |

The test: if someone would look at `open-questions.md` expecting to find it and it's not there, it belongs there. If it has been answered, remove it.

---

## Closing a question

When a question in `open-questions.md` is resolved:

1. **Remove it from `open-questions.md`.** The file tracks live, unresolved questions only. Resolved entries add noise.
2. **Check `current-development.md` for stale references.** If the question appeared in the Known blockers section, remove or update that entry.
3. **Check `build/build-sequence.md` for stale references.** If a build task was blocked on or referenced the question, mark it complete or update the task wording.
4. **Record the outcome if it is significant.** If the resolution involved a real product or architectural decision, add a brief entry to `docs/project/story.md` or create an ADR. A question about a keyboard shortcut does not need an ADR. A question about the data model does.

---

## Reviewing `current-development.md` for staleness

`current-development.md` is the lowest-authority document in the wiki — it reflects the current moment and goes stale quickly. Review it:
- Whenever an open question is resolved
- Whenever a milestone status changes
- At the start of each new month or build phase

The status table and Known blockers section are the most likely to drift. Treat any entry that has not been touched in four weeks as a candidate for removal or update.

---

## Status label vocabulary

Doc and decision `**Status:**` lines use a small controlled set. Pick the closest term; an em-dash qualifier may follow (e.g. `Final — non-negotiable`).

| Label | Meaning |
|---|---|
| **Final** | Decided, do not revisit |
| **Decided** | Chosen, could revisit if context changes |
| **In Progress** | Actively being worked out |
| **Directional** | Rough direction, not locked |
| **Deferred** | Pushed to a later version or phase |
