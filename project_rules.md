# OnThisDay.PH Marketing Website — Project Rules

## Project Overview

Static marketing website for [OnThisDay.PH](https://onthisday.ph), a civic tech platform helping Filipinos fact-check historical claims. Built with Astro as a zero-JS static site, separate from the main application.

## Tech Stack

- **Framework**: Astro 6.0.4
- **CSS**: Tailwind CSS v4 via `@tailwindcss/vite` — config in CSS, not a config file
- **Language**: TypeScript strict mode
- **Content**: Astro Content Collections + Zod
- **Deployment**: Vercel — production at `https://onthisday.ph`
- **Package Manager**: pnpm (required), Node.js >= 22.12.0

## Development Commands

```bash
pnpm dev      # Start dev server at http://localhost:4321
pnpm build    # Production build → dist/
pnpm preview  # Preview production build
```

No test suite. Verify changes with `pnpm build`.

## Repository Structure

```
src/
├── components/       # Reusable .astro components
├── layouts/
│   └── BaseLayout.astro   # Only layout — Header + Footer + SEOHead
├── pages/            # File-based routing
│   ├── index.astro   # Landing page
│   ├── about.astro
│   ├── 404.astro
│   └── blog/
│       ├── index.astro
│       └── [slug].astro
├── content/blog/     # Markdown blog posts
├── styles/global.css # Tailwind @theme + custom utilities
└── content.config.ts # Zod schema for blog posts
```

## Critical Conventions

### Components
- `.astro` files only in `src/components/`
- Always define `interface Props` in component frontmatter
- Zero client-side JS — static site, no hydration by default
- `BaseLayout.astro` is the **only** layout — never create another

### BaseLayout Props
```typescript
interface Props {
  title: string;       // required
  description: string; // required
  ogImage?: string;
  type?: 'website' | 'article';
  publishedDate?: string; // ISO string, blog posts only
}
```

### Tailwind CSS v4 — Critical
- Configuration is in `src/styles/global.css` under `@theme {}` — **never** use `tailwind.config.js`
- Brand color palette (always use for brand elements):
  - `ph-blue` = `#0038A8` / `ph-blue-dark` = `#002A7F`
  - `ph-red` = `#CE1126` / `ph-red-dark` = `#A80D1E`
  - `ph-yellow` = `#FCD116`
- Pre-defined utility classes (do not redefine): `.gradient-border-card`, `.animate-on-scroll`, `.animate-on-scroll-delay-1`, `.animate-on-scroll-delay-2`, `.animate-on-scroll-scale`, `.animate-pulse-glow`, `.animate-float`, `.animate-float-delay`, `.animate-count-up`

### Blog Posts — Content Collections
Frontmatter schema enforced by Zod. Missing fields cause build failure:
```yaml
---
title: "Post Title"
description: "One-sentence SEO summary"
date: "YYYY-MM-DD"
author: "OnThisDay.PH"  # optional, defaults to "OnThisDay.PH"
---
```
Slug = filename. Posts auto-appear in blog listing sorted by date descending.

### File Routing
```
src/pages/index.astro        → /
src/pages/about.astro        → /about
src/pages/blog/index.astro   → /blog
src/pages/blog/[slug].astro  → /blog/<slug>
src/pages/404.astro          → 404
```

## Rules — Do Not

- Add React, Vue, or Svelte — zero-JS is intentional
- Create `tailwind.config.js` — configuration belongs in `global.css`
- Create new layouts — only `BaseLayout.astro` exists and must remain the only one
- Hardcode secrets — `.env` files are gitignored
- Push directly to `main` — use pull requests

## Adding Features

**New page**: Create `src/pages/<name>.astro`, use `BaseLayout`, update `Header.astro` and `Footer.astro` nav links
**New component**: Create `src/components/<Name>.astro` with typed `interface Props`, use brand colors
**New blog post**: Create `src/content/blog/<slug>.md` with all required frontmatter fields

## Related Projects

- Main app (separate repo): Next.js 14 + Hono + Supabase at `https://app.onthisday.ph`
- Marketing site links to the app but shares no code with it
