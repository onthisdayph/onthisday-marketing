# OnThisDay.PH — Marketing Website

The marketing and SEO website for [OnThisDay.PH](https://onthisday.ph), a civic tech platform that presents modern Philippine history in meme format with verified sources.

## About the Project

**OnThisDay.PH** helps Filipinos remember what happened — and hold leaders accountable. Historical events are presented in a shareable meme format, with every entry backed by verified sources (news articles, government records, academic publications). The community submits, verifies, and curates the timeline together.

This repository contains the **marketing website**, a separate static site optimized for SEO and performance. The main application (Next.js 14 + Hono + Supabase) lives in a different repository.

### Key Features of the Platform

- **Timeline Feed** — Browse Philippine history day by day
- **Source Verification** — Every entry requires linked, verified sources
- **Community Voting** — Upvote important events and verify accuracy
- **Meme Format** — Designed for social media shareability
- **Free & Open Source** — Built by Filipinos, for Filipinos

## Tech Stack

| Layer | Technology |
|-------|-----------|
| Framework | [Astro](https://astro.build) (static-first, zero JS by default) |
| Styling | [Tailwind CSS v4](https://tailwindcss.com) with Philippine flag color palette |
| Content | Astro Content Collections (Markdown blog posts) |
| Deployment | [Vercel](https://vercel.com) (free Hobby tier) |
| Sitemap | `@astrojs/sitemap` (auto-generated) |

## Project Structure

```
src/
├── layouts/
│   └── BaseLayout.astro        # HTML shell, SEO head, header, footer
├── components/
│   ├── Header.astro            # Sticky nav with mobile menu
│   ├── Footer.astro            # Site footer
│   ├── Hero.astro              # Landing page hero
│   ├── HowItWorks.astro        # 4-step explainer
│   ├── FeatureCard.astro       # Reusable feature card
│   ├── MemeShowcase.astro      # Curated meme examples
│   ├── CTASection.astro        # Call-to-action banner
│   ├── FAQ.astro               # Accordion FAQ
│   ├── SEOHead.astro           # Dynamic meta/OG/JSON-LD
│   └── BlogCard.astro          # Blog post card
├── pages/
│   ├── index.astro             # Landing page
│   ├── about.astro             # About / mission
│   ├── 404.astro               # Not found
│   └── blog/
│       ├── index.astro         # Blog listing
│       └── [slug].astro        # Blog post
├── content/
│   └── blog/*.md               # Blog posts (Markdown)
├── content.config.ts           # Content collection schema
└── styles/
    └── global.css              # Tailwind + custom theme
```

## Getting Started

### Prerequisites

- [Node.js](https://nodejs.org) >= 22.12.0
- [pnpm](https://pnpm.io)

### Development

```bash
# Install dependencies
pnpm install

# Start dev server
pnpm dev
```

The site will be available at `http://localhost:4321`.

### Build

```bash
# Build for production
pnpm build

# Preview the build locally
pnpm preview
```

## Deployment

The site deploys to **Vercel** with the `@astrojs/vercel` adapter.

- **Production:** `https://onthisday.ph`
- **App:** `https://app.onthisday.ph` (separate repo)

Push to `main` to trigger a deploy.

## Writing Blog Posts

Add a new `.md` file in `src/content/blog/` with this frontmatter:

```markdown
---
title: "Your Post Title"
description: "A short description for SEO and cards."
date: "2026-03-14"
author: "OnThisDay.PH"
---

Your content here...
```

## Color Palette

The site uses the Philippine flag colors:

| Color | Hex | Usage |
|-------|-----|-------|
| Blue | `#0038A8` | Primary, headers, CTAs |
| Red | `#CE1126` | Accents, dates, secondary CTAs |
| Yellow | `#FCD116` | Highlights, buttons on dark backgrounds |

## License

Open-source civic tech for the Filipino people.
