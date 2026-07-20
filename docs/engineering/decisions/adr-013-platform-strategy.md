---
title: "ADR-013: Platform and mobile strategy"
parent: "Decision Records"
grand_parent: "Engineering"
nav_order: 13
layout: default
---

# ADR-013: Platform and mobile strategy

**Status:** Decided for v1 — mobile strategy deferred, options documented  
**Date:** July 2026

## Decision

Fosta v1 is Mac-only. Mobile (iPhone, iPad) is explicitly out of scope for v1. When mobile is addressed, the recommended path is a lightweight companion app in Swift sharing the Cloudflare backend — not a port of the Mac app.

The local-first, markdown-on-disk architecture is retained for v1. The cloud-first alternative was considered and documented below.

---

## Context

This decision was reached after revisiting whether the macOS-native approach was the right long-term platform strategy. The specific concerns were:

1. **Cross-device reach** — a Mac-only app cannot be used on iPhone, iPad, or by non-Mac users
2. **Scalability** — if mobile is added later, does the current architecture support it or require a full rebuild?
3. **Distribution** — a web app or PWA is simpler to distribute than a native app

These are legitimate concerns. This ADR documents the full trade-off analysis and the reasoning for the decision reached.

---

## How cross-platform tools actually work

Understanding how tools like Notion, Bear, and Craft handle multi-platform is useful context.

**Notion** is fundamentally a web application. Their servers store data in a proprietary database. The desktop "app" is an Electron wrapper — a browser rendering their web app with a native chrome around it. The iOS app is a separate native build. All clients talk to the same cloud backend. Markdown is an export format, not the storage format.

**Bear and Craft** take a different approach: separate native codebases for Mac and iOS (Swift/SwiftUI for iOS, AppKit/SwiftUI for Mac), sharing a sync backend (CloudKit or their own). The Mac app is the full experience; the iOS app is optimised for mobile workflows. They share data but not UI code.

**The key insight:** "one app everywhere" is usually a compromise. The best multi-platform tools tend to have purpose-built clients per platform sharing a common backend and data format.

---

## The cloud-first alternative — considered and set aside

The alternative to local-first is cloud-first: notes stored in a cloud database or cloud storage, accessed via API, with the app as a thin client. This is how Notion, Craft (optional), and most modern SaaS tools work.

### What cloud-first would give Fosta

- Cross-device from day one — Mac, iPad, iPhone, browser
- Simpler distribution — no App Store notarisation complexity for early versions
- Easier to add sharing and collaboration later
- No user-managed local files

### What cloud-first would cost

- **Auth required immediately** — user accounts, passwords, OAuth, session management. This is significant build complexity before a single note-taking feature is done.
- **API layer required** — every read and write goes through a backend. No more direct filesystem calls.
- **Server costs** — scale with users from day one, not after product-market fit.
- **Offline becomes complex** — local-first gives you offline for free. Cloud-first requires service workers, sync queues, conflict resolution, and careful engineering to support offline use.
- **You become responsible for user data** — backups, security, GDPR, data residency.
- **More build complexity, not less** — the simplicity gain from going cloud is on the user side (any device). The build side becomes significantly harder.

### The markdown portability question

The specific concern was not about abandoning cloud storage — it was about abandoning the markdown-on-disk architecture. The requirement was: *if a user leaves Fosta, they should be able to export their notes as real markdown files and open them in Obsidian or any other tool.*

This requirement is compatible with cloud storage. Notes can be stored as real `.md` files in Cloudflare R2 rather than on the local disk. The storage format remains markdown; the location changes from local disk to cloud storage. Export = zip of `.md` files. Zero lock-in.

**This option was noted but not adopted for v1.** The additional build complexity of auth, API, and cloud storage management is not justified before the core product is validated. It remains the most credible v2 path if cross-device becomes a confirmed user need.

---

## Mobile — what a Fosta iPhone/iPad app would actually look like

Before choosing a mobile technical approach, it's worth establishing what Fosta on mobile actually is. The five-view pipeline with canvases, three-panel editors, and tag clouds is designed for a large screen. Mobile is a different context, not a smaller Mac.

**Likely mobile scope:**

| View | Mobile? | Notes |
|---|---|---|
| Capture | ✅ Yes | Quick capture on mobile is a genuine need |
| Sort | ⚠️ Maybe | Tagging and triaging could work on iPad; phone is tight |
| Layer | ✅ Yes | Reading and writing notes on mobile makes sense |
| Track | ❌ Probably not | Timeline canvases on a 6" screen is a stretch |
| Share | ❌ Probably not | Composition is desktop work |

This is how Bear and Craft approach it — full experience on Mac, capture and reading on iPhone. Different products sharing a backend.

---

## Cross-platform technical options evaluated

### Option A — Tauri (current) + separate Swift iOS app

Mac: Tauri + React. iPhone/iPad: Swift/SwiftUI, built separately. Shared: Cloudflare backend and markdown files.

**Pros:** best native experience on both platforms, proven model (Bear, Craft).  
**Cons:** two separate codebases, requires learning Swift for mobile, significant additional build effort.

### Option B — React Native

One React codebase compiling to native UI on Mac, iPhone, iPad, and Android.

**Pros:** one codebase, React skills transfer, genuinely native feel on iOS.  
**Cons:** React Native macOS target is immature; complex desktop interactions (canvases, three-panel editors, drag-and-drop) are poorly supported on the macOS target. The desktop experience tends to feel like a ported mobile app.

### Option C — Capacitor (web app wrapped as native)

Web app wrapped in a native shell for iOS and Mac App Store distribution via Capacitor. UI is HTML/CSS/JS in a WebView.

**Pros:** one web codebase, App Store distribution, works across Apple devices.  
**Cons:** WebView on mobile feels less native; complex canvas and drag interactions can be sluggish; less access to native APIs than Option A.

### Option D — Progressive Web App

Web app installable to home screen on iPhone/iPad and dock on Mac.

**Pros:** one codebase, no App Store, instant updates.  
**Cons:** iOS Safari severely limits PWAs — no push notifications, storage limits, no background processing. Poor discoverability (manual "Add to Home Screen"). Not appropriate for a premium tool.

---

## Decision and rationale

**For v1:** Mac only, Tauri, local-first. The mobile and cross-device question is deferred.

**Rationale:**
- The Fosta experience is fundamentally a desktop experience. The pipeline, canvases, and three-panel editor earn their value on a large screen. Mobile is a companion, not the main event.
- Local-first is simpler to build than cloud-first. No auth, no API, no server costs, no sync conflicts. This matters for a solo developer building v1.
- The right mobile strategy will become clear once there are real users. The need for mobile capture may be met entirely by the existing Cloudflare work-access flow (browser form → Cloudflare → inbox).
- None of the current technical decisions prevent adding mobile later. The Cloudflare backend is already planned. A Swift iOS app sharing that backend is a well-understood path.

**If mobile becomes a confirmed v1 requirement:** the recommended path is a lightweight Swift capture + reading app for iPhone that talks to the Cloudflare backend. Share the data layer and format, not the UI code.

**If cross-device parity (full Fosta on all devices) becomes a requirement for v2:** the recommended path is to move to cloud markdown files in R2, add an API layer via Cloudflare Workers, and consider Capacitor as the app shell to share UI code across platforms. This would require a significant architectural revision and should be scoped as a distinct project.

---

## What this decision explicitly does not close off

- Adding a Swift iOS companion app in v1.1 or v2
- Moving to cloud markdown storage in v2
- Distributing Fosta via the Mac App Store (Tauri supports this)
- Adding Windows or Linux support via Tauri (same codebase, different build target)
