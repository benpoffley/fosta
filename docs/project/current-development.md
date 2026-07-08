# Current Development

**Status:** Updated regularly — lowest authority section  
**Current milestone:** Month 1 — Pre-code foundation

## Status overview

| Area | Status |
|---|---|
| Product brief | ✅ Complete |
| Lo-fi wireframes (all views) | ✅ Complete |
| Competitive research | ✅ Complete |
| Architecture decisions (ADRs 001–012) | ✅ Complete |
| Data model design | ✅ Complete |
| Product wiki (this repo) | ✅ Complete |
| Hi-fi Sort view (Figma) | 🔄 In progress |
| Three view name decisions | ⏳ Deferred to hi-fi phase |
| Dev environment setup | ⏳ Week 4 |
| First GitHub commit | ⏳ Week 4 |

## Current priority

**Hi-fi Sort view in Figma.** Sort locks the visual language for the entire app — typography, colour system, spacing, component patterns. Everything else follows from it. Do not start building before Sort hi-fi is resolved.

## Three naming decisions pending

Deferred to the hi-fi Figma phase. Will resolve when real screens and real copy make the right answer obvious.

- First tab: **Desk** vs **Base**
- Organisation view: **Sort** vs **Curate**
- Editor view: **Layer** vs **Work** vs **Develop**

## Known blockers

- Sort inbox interaction model not finalised — drag-to-tag vs click-to-select model still being resolved
- Three view names deferred

## Build sequence (high level)

See `../../build/build-sequence.md` for the full phased build plan.

| Phase | Timeline | Focus |
|---|---|---|
| 1 | Month 1 | Foundation — brief, wireframes, hi-fi Sort |
| 2 | Month 1 Week 4 | Dev environment, first commit |
| 3a | Months 2–4 | Capture + Sort |
| 3b | Months 4–5 | Cloudflare backend |
| 4 | Months 5–8 | Layer → Share → Track |
| v1.0 | Months 8–9 | Polish + launch |
