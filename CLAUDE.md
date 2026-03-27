# CLAUDE.md — Appligent AI Website

This file provides essential context for AI assistants working with this codebase.

## Project Overview

A single-page marketing website for **Appligent AI**, an AI automation and mobile development consultancy based in Zurich, Switzerland. Built with Astro 6 and Tailwind CSS 4, deployed to GitHub Pages at `https://appligent.ai`.

## Tech Stack

| Tool | Version | Role |
|------|---------|------|
| [Astro](https://astro.build) | ^6.1.0 | Static site framework and build system |
| [Tailwind CSS](https://tailwindcss.com) | ^4.2.2 | Utility-first styling |
| `@tailwindcss/vite` | ^4.2.2 | Tailwind Vite plugin (Tailwind 4 integration method) |
| TypeScript | via Astro | Type checking (strict mode) |
| GitHub Actions | — | CI/CD to GitHub Pages |
| Plausible Analytics | — | Privacy-focused web analytics |

**Node requirement:** `>=22.12.0`

## Repository Structure

```
appligent-website/
├── .github/workflows/deploy.yml  # CI/CD: build + deploy to GitHub Pages
├── public/
│   ├── CNAME                     # Custom domain: appligent.ai
│   ├── favicon.ico
│   └── favicon.svg
├── src/
│   ├── assets/                   # SVG images (background, astro template assets)
│   ├── components/
│   │   └── Welcome.astro         # Deprecated template component — do not use
│   ├── layouts/
│   │   └── Layout.astro          # Master HTML5 document wrapper (head, analytics, fonts)
│   ├── pages/
│   │   └── index.astro           # Main landing page (the entire website)
│   └── styles/
│       └── global.css            # Design tokens, base styles, reusable component classes
├── astro.config.mjs              # Astro config (site URL, Vite/Tailwind plugin)
├── package.json
└── tsconfig.json                 # Extends astro/tsconfigs/strict
```

## Development Commands

```bash
npm run dev       # Start dev server at localhost:4321 (hot reload)
npm run build     # Build static output to ./dist/
npm run preview   # Locally preview the production build
npm run astro     # Direct Astro CLI access (e.g., astro check)
```

No test runner, linter, or formatter is configured. Run `astro check` for TypeScript diagnostics.

## Key Architecture Decisions

### Static-only, zero client JavaScript
Astro sends pure HTML/CSS to the browser by default. There is no client-side JavaScript framework (no React, Vue, etc.). The only client-side JS is an inline `<script is:inline>` in `index.astro` for the lead-capture form (UI reset only — the form does not currently submit data to a backend).

### Single-page site
All content lives in `src/pages/index.astro`. The page has these sections in order:
1. **Nav** — sticky header with logo and CTA button
2. **Hero** — headline, subheadline, CTAs, stats badge
3. **Expertise** — 7 service pillars (grid of cards)
4. **Process** — 4-step engagement flow (Discover → Architect → Build → Support)
5. **Testimonials** — 3 client quotes
6. **CTA band** — gradient call-to-action
7. **Lead form** — email, company, name, location, needs, GDPR consent
8. **Footer**

### Tailwind CSS 4 integration
Tailwind 4 is imported differently from v3 — via `@import "tailwindcss"` at the top of `src/styles/global.css`, with no separate `tailwind.config.*` file. The Vite plugin `@tailwindcss/vite` handles everything.

### Layout component
`src/layouts/Layout.astro` wraps every page (currently just `index.astro`). It handles:
- `<head>` with charset, viewport, description meta
- Favicon links
- Google Fonts preconnect (Inter + Space Grotesk)
- Plausible Analytics script
- Body base classes: `bg-surface text-slate-900 antialiased`
- Smooth scrolling

## Design System

### Color Tokens (CSS custom properties in `global.css`)

| Variable | Value | Usage |
|----------|-------|-------|
| `--clr-bg` | `#f4f5f7` | Page background |
| `--clr-surface` | `#ffffff` | Card / component background |
| `--clr-text` | `#0b0d13` | Primary text |
| `--clr-muted` | `#4b4f5b` | Secondary / caption text |
| `--clr-border` | `#e0e3ea` | Borders |
| `--clr-accent` | `#00bfa6` | Primary action (teal) |
| `--clr-accent-dark` | `#00907d` | Hover state for accent |
| `--clr-highlight` | `#ff8a00` | Orange highlight |

### Typography

- **Body font:** Inter (400, 500, 600) — from Google Fonts
- **Heading font:** Space Grotesk (400, 500, 600) — from Google Fonts
- Headings (`h1`–`h3`) use `font-family: 'Space Grotesk', sans-serif`

### Reusable CSS Classes (defined in `global.css`)

| Class | Description |
|-------|-------------|
| `.btn-primary` | Teal filled button with hover shadow |
| `.btn-secondary` | Teal bordered transparent button |
| `.section-shell` | Responsive section padding (`py-20 md:py-24`) |
| `.card` | White card with shadow and rounded border |
| `.gradient-band` | Teal gradient section with white text |
| `.hero-visual` | Hero image container with gradient and border |
| `.bg-surface` | White background utility |

## Naming Conventions

- **Astro component files:** PascalCase (e.g., `Layout.astro`, `Welcome.astro`)
- **CSS classes:** kebab-case (e.g., `.btn-primary`, `.section-shell`)
- **Data attributes for analytics:** snake_case values (e.g., `data-event="cta_click"`, `data-location="hero"`)
- **Content arrays in frontmatter:** camelCase variable names (e.g., `services`, `steps`, `testimonials`)

## Deployment

Deployment is fully automated via GitHub Actions (`.github/workflows/deploy.yml`):

1. **Trigger:** Push to `main` branch, or manual `workflow_dispatch`
2. **Build:** `npm ci` → `npm run build` → uploads `./dist/` artifact
3. **Deploy:** GitHub Pages deployment from the artifact
4. **Domain:** Custom domain `appligent.ai` configured via `public/CNAME`
5. **Site base URL:** Set in `astro.config.mjs` as `site: 'https://appligent.ai'`

To deploy changes: merge to `main`. Feature branches do not auto-deploy.

## Analytics

Plausible.io analytics are embedded in `Layout.astro`. Analytics events use `data-event` and `data-location` attributes on interactive elements. No Google Analytics or cookies are used.

## Development Guidelines for AI Assistants

- **Do not add a backend or API routes** unless explicitly requested. The site is intentionally static.
- **Do not introduce client-side frameworks** (React, Vue, Svelte, etc.) without explicit instruction. Use Astro's built-in templating.
- **Extend global.css** for new reusable styles; use Tailwind utilities inline for one-off styles.
- **Add new sections** as clearly delimited HTML blocks inside `index.astro`, following the existing pattern of `<section id="..." class="section-shell ...">`.
- **Add new pages** as `.astro` files in `src/pages/` and wrap them with `<Layout>`.
- **Add new reusable components** as `.astro` files in `src/components/` using PascalCase names.
- **Do not modify `Welcome.astro`** — it is a deprecated template artifact and can be safely deleted.
- **Use CSS custom properties** (`var(--clr-accent)` etc.) for colors instead of hardcoding hex values in new code.
- **Do not add a `tailwind.config.*` file** — Tailwind 4 does not use one; configuration is done via CSS.
- **Preserve the GDPR consent checkbox** in the lead form — it is legally required for EU compliance.
- There are **no environment variables** currently in use. If adding secrets, use GitHub Actions secrets and Astro's `import.meta.env` pattern.
