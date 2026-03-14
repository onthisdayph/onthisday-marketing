# GEMINI.md — OnThisDay.PH Marketing Website

AI assistant guide for the `onthisday-marketing` repository.

## Project Overview

This is the **marketing website** for [OnThisDay.PH](https://onthisday.ph), a civic tech platform that helps Filipinos fact-check historical claims using dated evidence. The site is built as a static marketing front-end, separate from the main application (which lives in a different repository).

## Tech Stack

| Layer | Technology | Version |
|---|---|---|
| Framework | Astro | 6.0.4 |
| CSS | Tailwind CSS v4 (via `@tailwindcss/vite`) | 4.2.1 |
| Language | TypeScript (strict mode) | via tsconfig |
| Content | Astro Content Collections + Zod | built-in |
| SEO | `@astrojs/sitemap` | 3.7.1 |
| Deployment | Vercel (`@astrojs/vercel`) | 10.0.0 |
| Package Manager | **pnpm** (required) | — |
| Node.js | **>=22.12.0** (required) | — |

## Development Commands

```bash
pnpm dev        # Start local dev server (http://localhost:4321)
pnpm build      # Build for production → dist/
pnpm preview    # Preview production build locally
```

**No test suite exists.** Verification is done by running `pnpm build` and visual inspection.

## Repository Structure

```
onthisday-marketing/
├── src/
│   ├── components/       # Reusable Astro components
│   ├── layouts/
│   │   └── BaseLayout.astro   # Master layout (Header + Footer + SEOHead)
│   ├── pages/            # File-based routing (Astro)
│   │   ├── index.astro   # Landing page
│   │   ├── about.astro   # About page
│   │   ├── 404.astro     # Not found page
│   │   └── blog/
│   │       ├── index.astro    # Blog listing
│   │       └── [slug].astro   # Dynamic post template
│   ├── content/
│   │   └── blog/         # Markdown blog posts (*.md)
│   ├── styles/
│   │   └── global.css    # Tailwind import + custom CSS utilities
│   └── content.config.ts # Content Collections schema (Zod)
├── public/               # Static assets (favicon, robots.txt)
├── astro.config.mjs      # Astro config (site URL, plugins, Vercel adapter)
├── tsconfig.json         # TypeScript config (extends astro/strict)
└── package.json
```

## Key Conventions

### Component Architecture

- All components are `.astro` files in `src/components/`
- Zero client-side JavaScript by default — this is a static site
- Components receive typed props via the frontmatter `interface Props` pattern
- `BaseLayout.astro` is the only layout; all pages use it

### BaseLayout Props

```typescript
interface Props {
  title: string;
  description: string;
  ogImage?: string;
  type?: 'website' | 'article';
  publishedDate?: string;  // ISO string, used for blog posts
}
```

Always pass `title` and `description` when using `BaseLayout`. For blog posts, also set `type="article"` and `publishedDate`.

### Styling — Tailwind CSS v4

- Tailwind v4 is loaded via the Vite plugin, **not** a `tailwind.config.*` file
- Theme customization is done in `src/styles/global.css` inside `@theme {}`
- **Philippine flag color palette** (use these for brand elements):
  - `ph-blue`: `#0038A8` (and `ph-blue-dark: #002A7F`)
  - `ph-red`: `#CE1126` (and `ph-red-dark: #A80D1E`)
  - `ph-yellow`: `#FCD116`
- Custom utility classes defined in `global.css`:
  - `.gradient-border-card` — card with animated Philippine-flag gradient border on hover
  - `.animate-on-scroll` / `.animate-on-scroll-delay-1` / `.animate-on-scroll-delay-2` — scroll-triggered fade-up
  - `.animate-on-scroll-scale` — scroll-triggered scale-in
  - `.animate-pulse-glow` — yellow pulsing glow for CTAs
  - `.animate-float` / `.animate-float-delay` — floating decorative elements
  - `.animate-count-up` — scroll-triggered counter animation

### Content Collections — Blog Posts

Blog posts live in `src/content/blog/*.md`. The schema (enforced by Zod in `content.config.ts`) requires:

```yaml
---
title: "Post Title"
description: "One-sentence summary for listings and SEO"
date: "2026-03-14"     # ISO date string YYYY-MM-DD
author: "OnThisDay.PH" # optional, defaults to "OnThisDay.PH"
---
```

- The `slug` is derived from the filename (e.g., `my-post.md` → `/blog/my-post`)
- Blog listing (`blog/index.astro`) sorts posts by `date` descending (newest first)
- All frontmatter fields must be present or the build will fail (Zod validation)

### SEO

- `SEOHead.astro` handles all meta tags: OpenGraph, Twitter Card, JSON-LD, canonical URLs
- The canonical base is `https://onthisday.ph` (set in `astro.config.mjs`)
- Sitemap is auto-generated on build by `@astrojs/sitemap`
- Always provide a meaningful `description` (used as `og:description` and `<meta name="description">`)

### File Routing

Astro uses file-based routing under `src/pages/`:
- `index.astro` → `/`
- `about.astro` → `/about`
- `blog/index.astro` → `/blog`
- `blog/[slug].astro` → `/blog/<slug>` (dynamic, driven by Content Collections)
- `404.astro` → custom 404 page

## Deployment

- Platform: **Vercel** (auto-deploy on push to `main`)
- Production URL: `https://onthisday.ph`
- The Vercel adapter is configured in `astro.config.mjs`
- Build output goes to `dist/` (gitignored)

## What to Avoid

- **Do not add a JavaScript framework** (React, Vue, Svelte) unless absolutely required. The site is intentionally zero-JS static.
- **Do not use `tailwind.config.js`** — Tailwind v4 configuration lives in `global.css` under `@theme {}`.
- **Do not add devDependencies for testing** without confirming with the team — no test infrastructure exists.
- **Do not hardcode secrets** — `.env` files are gitignored; no secrets belong in source.
- **Do not create new layouts** — `BaseLayout.astro` is the single layout for all pages.
- **Do not push to `main` directly** — deploy via pull request.

## Adding New Features

### New Page
1. Create `src/pages/<name>.astro`
2. Import and use `BaseLayout` with appropriate `title` and `description`
3. Add navigation link to `Header.astro` and `Footer.astro` if needed

### New Component
1. Create `src/components/<ComponentName>.astro`
2. Define a typed `interface Props` in the frontmatter
3. Use Philippine flag colors (`ph-blue`, `ph-red`, `ph-yellow`) for brand consistency

### New Blog Post
1. Create `src/content/blog/<slug>.md`
2. Include all required frontmatter fields (`title`, `description`, `date`)
3. The post appears automatically in the blog listing (sorted by date)

## Related Projects

- **Main App** (separate repo): Next.js 14 + Hono + Supabase, deployed at `https://app.onthisday.ph`
- This marketing site links to the app but does not share code with it

## Keeping This File Current

When any change affects the repository structure or conventions, **update all AI memory files** to reflect it. Outdated memory files cause AI assistants to give wrong advice.

Update when:
- **Pages or routes change** — update the routing table and repository structure
- **Components added/removed** — update the structure and conventions sections
- **Dependencies change** — update the Tech Stack table
- **Scripts change** — update the Development Commands section
- **CSS tokens or utility classes change** — update the Tailwind CSS section
- **Content schema changes** — update the Blog Posts / Content Collections section
- **Conventions change** — update the relevant section

All AI memory files must be kept in sync:
- `CLAUDE.md` (Claude Code)
- `AGENTS.md` (OpenAI Codex / agents)
- `GEMINI.md` (Gemini CLI)
- `.github/copilot-instructions.md` (GitHub Copilot)
- `.cursor/rules/project.mdc` (Cursor)
- `.windsurfrules` (Windsurf)
- `.rules` + `project_rules.md` (Trae)
