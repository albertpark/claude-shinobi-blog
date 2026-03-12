# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Commands

```bash
npm run dev        # Start dev server with Turbopack
npm run build      # Build for production
npm run lint       # Run ESLint
npm run test       # Run tests with Vitest
npm run test:ui    # Run tests with Vitest UI dashboard
```

To run a single test file: `npx vitest run src/path/to/file.test.tsx`

## Environment Variables

`HYGRAPH_ENDPOINT` — required for GraphQL data fetching from the Hygraph CMS.

## Key Technologies

| Technology | Version | Purpose |
|---|---|---|
| Next.js | 15.x | App Router, SSR/ISR, routing |
| React | 19.x | UI rendering |
| TypeScript | 5.x | Type safety |
| Tailwind CSS | 4.x | Styling via `@theme inline` CSS variables |
| Vitest | 3.x | Unit testing with jsdom environment |
| Testing Library | latest | React component testing |
| Hygraph | — | Headless CMS (GraphQL API) |
| DOMPurify | 3.x | HTML sanitization for CMS content |
| lucide-react | latest | Icon library |

## Architecture Overview

**Data layer:** Blog content is fetched from Hygraph (headless CMS) via GraphQL using raw `fetch()` calls. Queries are defined in `src/lib/queries.ts`. Pages use ISR with `next: { revalidate: 3600 }`. Blog post HTML content is sanitized via DOMPurify (`src/lib/sanitize.ts`) before rendering.

**Component library:** Reusable UI components live in `src/components/ui/` (Button, Avatar, Card, Icon, Modal). Each supports `variant` (primary, secondary, success, warning, danger) and `size` (sm, md, lg, xl) props. The `/preview` route is a live showcase of all components.

**Theme system:** Light/dark mode is managed by `src/hooks/useTheme.ts`, persisted to localStorage. CSS custom properties in `src/app/globals.css` define the full color palette using Tailwind 4's `@theme inline` syntax. The `DarkModeToggle` component lives in the root layout.

**Path alias:** `@/*` maps to `src/*`.

## Conventions

- When creating a new page under `src/app/`, always add a link to it in the header in `src/app/layout.tsx`.

## Project Structure

```
src/
├── app/                        # Next.js App Router pages
│   ├── layout.tsx              # Root layout with navbar and theme toggle
│   ├── page.tsx                # Home page
│   ├── about/page.tsx          # About page
│   ├── blog/
│   │   ├── page.tsx            # Blog listing (fetches all posts from Hygraph)
│   │   └── [slug]/page.tsx     # Dynamic blog post page (fetches single post)
│   ├── preview/page.tsx        # Component showcase / design system preview
│   └── globals.css             # CSS variables, theme tokens, font imports
├── components/
│   ├── ui/                     # Reusable UI components (Button, Avatar, Card, Icon, Modal)
│   ├── BlogSidebar.tsx
│   └── DarkModeToggle.tsx
├── hooks/
│   └── useTheme.ts             # Dark/light mode toggle with localStorage persistence
├── lib/
│   ├── queries.ts              # GraphQL queries (GET_BLOG_POSTS, GET_SINGLE_POST)
│   ├── sanitize.ts             # DOMPurify HTML sanitization utility
│   └── types.ts                # Shared TypeScript types
└── test/
    └── setup.ts                # Vitest + Testing Library setup
```
