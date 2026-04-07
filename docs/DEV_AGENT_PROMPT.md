# DEV_AGENT_PROMPT — newpush-labs-pitchdeck

> NewPush Labs investor / conference pitch deck built with **Slidev 0.50-beta**, Vue 3, and multiple themes (academic, dracula, penguin, seriph). Deployed to **Netlify** and **Vercel**.

---

## 1. Orientation — Read the Docs

Before touching any file, read and internalise:

| Document | Why |
|----------|-----|
| `README.md` | Project purpose and slide structure |
| `slides.md` | Main slide deck — **source of truth** for content |
| `pages/imported-slides.md` | Imported/partial slide content |
| `vite.config.js` | Vite/Slidev plugin configuration |
| `package.json` | Scripts: `dev`, `build`, `export` |
| `netlify.toml`, `vercel.json` | Deploy settings for both platforms |
| `theme/` | Custom theme overrides (layoutHelper.ts, package.json) |

### Tech Stack

- **Slidev 0.50-beta** with `@slidev/cli`
- **Vue 3** components (`components/Counter.vue`)
- **Themes**: `slidev-theme-academic`, `slidev-theme-dracula`, `slidev-theme-penguin`, `@slidev/theme-seriph`
- **unplugin-icons** for icon sets, **vite-svg-loader** for inline SVGs
- **TypeScript** snippets (`snippets/external.ts`)
- Deployed to **Netlify** and **Vercel** simultaneously

### Key Patterns

- Slides live in `slides.md` using Slidev's YAML frontmatter per slide
- `slides_.md` and `temp-slides.md` are draft/alternate versions — **do not delete**
- `pages/imported-slides.md` is pulled into the main deck via Slidev's `src:` directive
- Images go in `images/` and are referenced with relative paths
- Custom theme in `theme/` extends one of the installed themes — changes here affect all slides
- `.npmrc` contains registry config — respect it

---

## 2. Plan — Write a Plan

Before writing code, create a plan in `docs/IMPLEMENTATION_PLAN.md`:

1. **What** — the feature, fix, or content change
2. **Which slides** — list affected slide numbers and their content headings
3. **Theme impact** — does this require theme changes or new layout components?
4. **Deploy verification** — Netlify and Vercel preview URLs to check
5. **Visual check** — export to PDF and review before merging

---

## 3. Documentation — Write User Docs First

Update documentation **before** implementing:

- `README.md` — update if adding new scripts, themes, or deployment targets
- Slide speaker notes (in Slidev `<!-- notes -->` blocks) — keep current
- If adding a new theme or component, document usage in `README.md`

---

## 4. Tests — Write Tests First

Slidev has limited test infrastructure, so focus on:

- **Build test**: `npm run build` must complete without errors
- **Export test**: `npm run export` must produce a valid PDF
- **Component test**: If adding Vue components in `components/`, add a basic unit test or at minimum verify the dev server renders correctly
- **Link/image check**: Verify all image paths in `slides.md` resolve to files in `images/`

---

## 5. Code — Write the Code

### Slide Content Rules

- One slide = one `---` separator block in `slides.md`
- Use YAML frontmatter per slide for layout, theme, class overrides
- Keep text concise — this is a pitch deck, not documentation
- Use `{.className}` for Windi CSS utility classes on elements
- SVG logos and icons use `vite-svg-loader` — import as Vue components

### Component Rules

- Vue 3 Composition API (`<script setup>`) for all new components
- Place in `components/` — Slidev auto-imports them
- Use unplugin-icons syntax: `<icon-name />` components

### Theme Rules

- Custom overrides go in `theme/` directory
- `layoutHelper.ts` provides layout utilities — extend, don't replace
- Test with all configured themes before committing (switch frontmatter `theme:` value)

---

## 6. Test the Code — Verify Everything

```bash
npm run dev          # Visual check in browser
npm run build        # Production build must succeed
npm run export       # PDF export must produce valid file
```

- Check both Netlify and Vercel preview deploys after push
- Verify on multiple screen sizes (Slidev is viewport-dependent)

---

## Branch Workflow

```
feature/* ──► develop ──► main
```

- All work on `feature/*` branches off `develop`
- PR to `develop` for review
- `develop` → `main` via PR only (enforced by CI)
- Netlify/Vercel deploy from `main`
