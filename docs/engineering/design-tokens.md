---
title: "Design Tokens"
parent: "Engineering"
nav_order: 6
layout: default
---

# Design Tokens

**Status:** In Progress  
**Note:** Values below are candidates from hi-fi HTML prototyping (Sort view, dark + light), not from Figma, and **not yet finalised**. Two work-in-progress visual directions exist side by side — see `docs/views/sort.md` — and neither has been chosen. Do not hardcode these values into components until one direction is confirmed.

---

## Colours

Two candidate palettes exist, extracted directly from the two current prototypes: `assets/prototypes/sort-hifi-darkmode.html` and `sort-hifi-lightmode.html`. Both share the same token names; only the values differ.

### Action colours (Sort interaction model)

These four names appear throughout the Sort spec to describe tag chip states — see `docs/views/sort.md`.

| Name | Role | Dark value | Light value |
|---|---|---|---|
| sage | Add / positive action | `#8BAA8F` | `#4B7355` |
| gold (amber) | Extend / completing something | `#CBA45A` | `#8C6A1E` |
| rose | Remove / destructive action | `#C57C6E` | `#96453A` |
| ring (cream/ink) | Neutral selection indicator (at rest) | `rgba(243,237,225,0.6)` | `rgba(33,29,24,0.45)` |

Note the light-mode accents are deliberately deepened/more saturated versions of their dark-mode counterparts — pastel tints that read clearly on a near-black ground disappear against a light beige ground, so contrast had to be re-tuned per direction rather than just inverted.

### Ground / surface palette — dark mode

| Token | Role | Value |
|---|---|---|
| green (bg) | App background (ground) | `#2A342A` |
| green-hi | Raised surfaces (header, floating nav) | `#313D31` |
| green-2 | Recessed (inputs, count pills) | `#232C23` |
| green-card | Card / chip surface | `#323F32` |
| green-card-h | Card / chip hover | `#3B493B` |
| rule | Hairline rule | `#47543F` |
| rule-soft | Fainter hairline | `#3A463A` |
| cream (text) | Primary text | `#F3EDE1` |
| cream-2 | Secondary text | `#CDC7B6` |
| cream-3 | Tertiary text | `#929A85` |
| clay (accent) | Primary CTA (Capture, active tab) | `#C56A45` |
| clay-2 | Clay hover | `#D07F55` |
| shadow | Elevation shadow | `0 14px 40px rgba(0,0,0,0.32)` |

### Ground / surface palette — light mode

Elevation direction inverts correctly for light mode: raised surfaces are *lighter* than the ground (not darker, as in dark mode), recessed surfaces are *darker*.

| Token | Role | Value |
|---|---|---|
| green (bg) | App background (ground) | `#D6CBB5` |
| green-hi | Raised surfaces (header, floating nav) | `#E6DECB` |
| green-2 | Recessed (inputs, count pills) | `#C7B99F` |
| green-card | Card / chip surface | `#E0D8C4` |
| green-card-h | Card / chip hover | `#EAE3D2` |
| rule | Hairline rule | `#B3A588` |
| rule-soft | Fainter hairline | `#C4B79C` |
| cream (text) | Primary text — near-black ink | `#211D18` |
| cream-2 | Secondary text | `#55493C` |
| cream-3 | Tertiary text | `#8C7F68` |
| clay (accent) | Primary CTA (Capture, active tab) | `#B4552F` |
| clay-2 | Clay hover | `#9C4726` |
| shadow | Elevation shadow (softer, warm-toned) | `0 14px 34px rgba(40,34,24,0.16)` |

---

## Typography

Two typefaces are used throughout the product, unaffected by the dark/light palette choice.

### Playfair Display — display headings

Used for h1, h2, h3, panel labels ("Notes", "Tags" in Sort), note/card titles, and blockquotes. Italic by default for display use.

| Token | Value |
|---|---|
| Font family | Playfair Display, Georgia, serif |
| h1 size | 2.2rem |
| h1 weight | 300 |
| h2 size | 1.5rem |
| h2 weight | 400 |
| h3 size | 1.15rem |
| h3 weight | 400 |
| Panel label size (Sort) | ~22–30px in the current prototypes — not yet locked |
| Panel label weight | 500 |

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
| Control / input size | ~11–13px in the current prototypes — not yet locked |

---

## Next steps

This document should be updated once one of the two visual directions (dark/light) — or a further-refined descendant of either — is actually chosen as final. At that point:
- Remove the unchosen palette's table entirely, or keep both if Fosta ends up supporting a genuine user-facing light/dark toggle (not yet decided — see `docs/foundations/settings.md`, "What is deliberately not here (yet)")
- Confirm exact type sizes for panel labels and controls, currently approximate
- Record any additional tokens introduced later (border radii, spacing scale) — most are already implicit in the prototypes but not yet named as tokens
