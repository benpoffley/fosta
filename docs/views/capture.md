# Capture

**Status:** Final  
**Type:** Global action — not a view, not a tab

## What it is

Capture is the intake mechanism for the entire pipeline. It is a Foundations-level capability available from every view. Notes always land in `Inbox/` regardless of where they were captured from.

The expanded capture state is the Quick Look modal — they are the same surface. See `../foundations/quick-look.md`.

## Behaviour by view

| View | Capture bar behaviour |
|---|---|
| Desk / Base | Always visible, persistent at bottom of canvas |
| All other views | Transient — animates in from + button on click, collapses after idle timeout, stays open once user starts typing |

## Triggers

- Keyboard shortcut (⌘N or similar — TBD)
- Persistent + button in the bottom toolbar (hidden on Desk where the bar is always visible)

## What happens on capture

1. User types in the capture bar / Quick Look modal
2. On save: note created as Markdown file in `Inbox/`
3. SQLite index updated
4. Capture surface closes (or stays open if user triggered "Open in Layer")

## Capture from Desk

On Desk, the capture bar sits persistently at the bottom of the canvas. The + button is hidden from the toolbar nav on the Desk view.

## The Capture / Quick Look connection

The capture flow ends in the Quick Look modal (note variant). This is not a separate component — it is Quick Look. The full editing surface, autosave via `saveNote()`, and "Open in Layer" escape hatch are all inherited from Quick Look.
