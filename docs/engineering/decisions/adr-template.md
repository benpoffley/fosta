---
title: "ADR Template"
parent: "Decision Records"
grand_parent: "Engineering"
nav_order: 14
layout: default
---

# ADR Template

This is a process document, not a decision record. Use it to create new ADRs.

---

## When to create an ADR

Create an ADR when a decision:
- Affects the core architecture, data model, or technology choices
- Cannot easily be reversed without significant rework
- Has meaningful alternatives that were considered and rejected
- Future developers (or AI agents) would benefit from understanding the reasoning, not just the outcome

Do not create an ADR for implementation details, styling choices, or decisions that can be changed without structural impact. Use `docs/project/open-questions.md` for unresolved questions and a story entry in `docs/project/story.md` for significant product decisions.

---

## File naming and location

Files live in `docs/engineering/decisions/`. Name them `adr-NNN-short-slug.md` where NNN is the next available number (zero-padded to three digits). Update `docs/engineering/decisions/index.md` to add the new entry.

---

## Template

Copy the block below into a new file. Fill in every section — leave none blank.

```markdown
---
title: "ADR-NNN: Short title"
parent: "Decision Records"
grand_parent: "Engineering"
nav_order: NNN
layout: default
---

# ADR-NNN: Full decision title

**Status:** Final  
**Date:** Month YYYY  
**Last reviewed:** Month YYYY (optional — add if revisited after initial write)

## Decision

One or two sentences. State plainly what was decided.

---

## Context

Why did this decision need to be made? What constraints or requirements shaped the choice? Keep this factual.

---

## Why [chosen option] over the alternatives

| Option | Decision | Reason |
|---|---|---|
| **[Chosen option]** | ✅ Chosen | Brief reason |
| **[Alternative A]** | ❌ Rejected | Brief reason |
| **[Alternative B]** | ❌ Rejected | Brief reason |

---

## What this gives us

- Bullet list of concrete benefits

---

## What this does not give us

- Bullet list of known limitations or things explicitly out of scope

---

## Why not to revisit

One paragraph. Explain what would have to change for this decision to be reconsidered, and why the cost of changing it is high. Be honest — if this is genuinely final, say so and explain why.
```
