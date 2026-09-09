---
title: "Design Tokens"
parent: "Engineering"
nav_order: 6
layout: default
---

# Design Tokens

**Status:** In Progress  
**Note:** This document is a placeholder. Values marked TBD will be confirmed once the Sort hi-fi Figma file is complete. Do not hardcode values in components until this doc is finalised.

---

## Colours

Named colours used across the product specs. Hex values are taken from the wiki's own visual system as a starting reference — final in-app values must be confirmed from the Sort hi-fi Figma work.

### Action colours (Sort interaction model)

These four names appear throughout the Sort spec to describe tag orb states.

| Name | Role | Value |
|---|---|---|
| sage | Add / positive action | TBD — confirm from Sort hi-fi |
| amber | Extend / completing something | TBD — confirm from Sort hi-fi |
| rose | Remove / destructive action | TBD — confirm from Sort hi-fi |
| cream | Neutral selection indicator (at rest) | TBD — confirm from Sort hi-fi |

### Dark background palette

The app uses a warm near-black background system. These values are established in the wiki theme and serve as a provisional reference.

| Token | Role | Current wiki value |
|---|---|---|
| bg | App background | `#0E0C0B` |
| bg-2 | Panel | `#161311` |
| bg-3 | Card / sidebar | `#211C1A` |
| bg-4 | Hover | `#2A2320` |
| rule | Hairline rule | `#302825` |
| rule-2 | Active rule | `#443C38` |
| cream (text) | Primary text | `#EAE0D0` |
| cream-2 | Secondary text | `#C0AA94` |
| cream-3 | Tertiary text | `#967E6E` |
| sage (accent) | Accent green / links | `#7AAF8A` |
| gold | Aged brass / active border | `#C8A055` |

---

## Typography

Two typefaces are used throughout the product.

### Playfair Display — display headings

Used for h1, h2, h3, panel labels ("Notes", "Tags" in Sort), and blockquotes. Italic by default for display use.

| Token | Value |
|---|---|
| Font family | Playfair Display, Georgia, serif |
| h1 size | 2.2rem |
| h1 weight | 300 |
| h2 size | 1.5rem |
| h2 weight | 400 |
| h3 size | 1.15rem |
| h3 weight | 400 |
| Panel label size (Sort) | TBD — confirm from Sort hi-fi |
| Panel label weight | TBD — confirm from Sort hi-fi |

### Inter — UI and body

Used for body text, navigation, labels, h4–h6, and all UI controls.

| Token | Value |
|---|---|
| Font family | Inter, -apple-system, BlinkMacSystemFont, sans-serif |
| Body size | 0.9rem |
| Body weight | 300 |
| Line height | 1.75 |
| UI label size | 0.75rem |
| UI label weight | 500 |
| UI label tracking | 0.08em (uppercase) |
| Control / input size | TBD — confirm from Sort hi-fi |

---

## Next steps

This document should be updated as soon as the Sort hi-fi Figma file reaches a stable state. At that point:
- Confirm all TBD colour values against the Figma file
- Record any additional tokens introduced during hi-fi (shadows, border radii, spacing scale)
- Update relevant view specs if any colour names changed during hi-fi
