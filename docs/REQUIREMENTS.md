# REQUIREMENTS — newpush-labs-pitchdeck

Canonical specification for the **NewPush Labs** pitch deck. This file is the source of truth for what the deck must communicate, how it must build and deploy, and what acceptance criteria a change must satisfy before merging.

Companion documents:

- `docs/DEV_AGENT_PROMPT.md` — workflow, branching, and agent rules.
- `slides.md` — the deck content itself.
- `README.md` — quick start.

---

## 1. Project Overview

NewPush Labs is one of three NewPush brands: it is the **lab / experimentation playground** offering. The pitch deck is the public, slide-based explanation of that offering and is used for investor conversations, conference talks, and partner introductions.

The deck must answer four questions in sequence:

1. **What problem are we solving?** — setting up an IT lab is slow, painful, and full of yak-shaving.
2. **Why now?** — accelerating tech change (AI, workflow automation, security tooling) makes "always have a current lab" a moving target.
3. **What is NewPush Labs?** — pre-configured, opinionated lab stacks (Workflow Lab, AI Lab, Web Dev Lab, …) that drop a working environment in front of the user.
4. **What do I do next?** — clear CTA pointing to `labs.newpush.com` and the public GitHub org.

The deck is **NewPush Labs branded**, not MikroVPS or Project NoéMI. Cross-brand references are allowed when intentional (e.g. mentioning Project NoéMI as a sister AI offering), but the deck does not pretend to be the marketing site for those brands.

## 2. Functional Requirements

### 2.1 Content Structure (required slides, in order)

| # | Slide | Purpose |
|---|---|---|
| 1 | Title / hero | Establish brand, set tone, link out to GitHub + `labs.newpush.com` |
| 2 | Problem framing | "Tired of spending hours configuring your IT lab?" |
| 3 | Empathy beat | "We've been there too." |
| 4 | Problem expansion | Why lab setup is overwhelming |
| 5 | Promise | "There's a better way" |
| 6 | Vision | "What if your IT lab were …" (bullet list of properties) |
| 7 | Imagine | Forward-looking framing |
| 8 | Solution intro | "Meet NewPush Labs" |
| 9 | Solution benefits | "With NewPush Labs you get …" |
| 10 | Lab Stacks — definition | What a "lab stack" is |
| 11 | Lab Stacks — examples | Workflow Lab, AI Lab, Web Development Lab |
| 12 | Use cases | What you can do |
| 13 | CTA | "Take the next step" — sign up / contact |
| 14 | Learn more | Documentation + GitHub links |

The deck **MUST stay roughly within this arc**. Adding, removing, or reordering top-level sections requires updating this table in the same PR as the slide change.

### 2.2 Imported / shared content

- `pages/imported-slides.md` is the canonical place for slide fragments reused across the deck. Inline new fragments in `slides.md` only if they are deck-specific.
- `slides_.md` and `temp-slides.md` are kept as **draft / alternate versions**. Do not delete; do not include them in the production build.

### 2.3 Theming

- Default deck theme is the local `./theme` directory, which extends one of the installed themes.
- Installed themes that MUST keep working (deck must build cleanly when `theme:` frontmatter is switched to any of these):
  - `@slidev/theme-default`
  - `@slidev/theme-seriph`
  - `slidev-theme-academic`
  - `slidev-theme-dracula`
  - `slidev-theme-penguin`
- Any new theme dependency added to `package.json` MUST be exercised in at least one slide or removed.

### 2.4 Assets

- All images live in `images/`. No remote-only image references in production slides (background URLs on the title slide are tolerated but should have a local fallback).
- SVG logos and icons load via `vite-svg-loader` and are imported as Vue components.
- Icon packs are accessed through `unplugin-icons` syntax (`<carbon-edit />`, `<carbon-logo-github />`, …).

### 2.5 Components

- New Vue components live in `components/` and use the Vue 3 Composition API with `<script setup>`.
- Slidev auto-imports components from `components/`; do not add manual imports in slides.

## 3. Non-Functional Requirements

### 3.1 Build & Export

- `npm run build` MUST complete with exit code 0 against a clean `node_modules`.
- `npm run export` MUST produce a valid PDF of the full deck.
- `npm run dev` MUST start the Slidev dev server on the default port and render slide 1 without console errors.

### 3.2 Deployment

The deck is deployed simultaneously to two providers:

| Provider | Config | Build command | Publish dir |
|---|---|---|---|
| Netlify | `netlify.toml` | `npm run build` | `dist` |
| Vercel | `vercel.json` | `npm run build` | `dist` |

Both providers MUST stay green on every merge to `main`. The `[[redirects]]` SPA fallback in `netlify.toml` and the equivalent `rewrites` in `vercel.json` are load-bearing — do not remove without replacing.

### 3.3 Node / package manager

- Node version pinned in `netlify.toml`: **20**. Vercel inherits the same major. Bumping the Node major requires updating `netlify.toml` in the same PR.
- `package-lock.json` is the lockfile of record. Do not introduce a second lockfile (no `pnpm-lock.yaml`, no `yarn.lock`).

### 3.4 Performance / size

- The exported PDF should remain reviewable in browser PDF viewers (target: under ~25 MB). Compress large images before adding them.
- No video embeds in the production deck; link out instead.

### 3.5 Accessibility & readability

- Body copy uses Roboto; code uses Fira Code. Stick to these unless the deck design changes globally.
- Slides target presentation viewing — text must be legible at 1080p projector resolution. Avoid <16pt body text in exported PDF.

## 4. Integration Requirements

This is a static deck; it has no runtime integrations. External touch points are:

| Touch point | Where | Notes |
|---|---|---|
| `labs.newpush.com` | Title slide, "Learn more" CTA | Public marketing site for NewPush Labs |
| `github.com/newpush-labs/newpush-labs` | Title slide, footer | Public source org link |
| `github.com/newpush/newpush-labs` | "Learn more" slide | Legacy mirror; verify before each launch that the link still resolves |
| Prismic CDN | Title-slide background image | If the asset moves, replace with a local image in `images/` |

## 5. Branch & PR Workflow

Per `docs/DEV_AGENT_PROMPT.md`:

```
feature/* (or docs/*) ──► develop ──► main
```

- Work on `feature/*` or `docs/*` branches off `develop`.
- PR into `develop` for review.
- `develop` → `main` via PR (Netlify / Vercel deploy from `main`).
- Date stamps in this file follow the org-wide `YYYY-MM-DD` / `YYYY-MM-DD-XXXX` convention.

## 6. Current Status & Implementation Gaps

As of 2026-05-21:

- [x] Slidev 0.50-beta deck builds and deploys to Netlify + Vercel.
- [x] Core 14-slide arc (problem → solution → CTA) is present in `slides.md`.
- [x] `docs/DEV_AGENT_PROMPT.md` exists and is authoritative for workflow.
- [ ] `docs/REQUIREMENTS.md` — **this file** — introduced by issue #2.
- [ ] Automated PDF-export smoke test in CI (currently a manual `npm run export`).
- [ ] Image-path lint (verify every `slides.md` image reference resolves under `images/`).
- [ ] Decision on the long-term fate of `slides_.md` / `temp-slides.md` (keep as drafts vs. archive vs. delete).
- [ ] Verification that the legacy `github.com/newpush/newpush-labs` link in the "Learn more" slide still resolves; if not, retarget to `newpush-labs/newpush-labs`.

## 7. Acceptance Criteria for Changes

A PR is mergeable when:

1. `npm run build` succeeds locally.
2. `npm run export` produces a PDF (attached to the PR if the content arc changed).
3. The Netlify and Vercel preview deploys both succeed and have been visually spot-checked.
4. If content sections were added / removed / reordered, the table in §2.1 is updated in the same PR.
5. If a new theme or major dependency was added, it is either used in a slide or omitted.
6. Slide speaker notes (`<!-- notes -->`) stay coherent with the visible content.
