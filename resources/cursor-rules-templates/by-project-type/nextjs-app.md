# Next.js App Rules

> **TL;DR**: Rules for Next.js projects — App Router, React Server Components, Tailwind CSS, TypeScript strict mode.

## Rules Set (Copy to `.cursor/rules/`)

### nextjs-overview.mdc
```markdown
---
description: Next.js project conventions
globs: ["*.ts", "*.tsx", "*.js", "*.jsx"]
alwaysApply: false
---

# Next.js Conventions

## App Router (Next.js 13+)
- Use app/ directory (NOT pages/)
- page.tsx for routes, layout.tsx for shared layouts
- loading.tsx for loading states, error.tsx for error boundaries
- Use Server Components by default — add 'use client' only when needed
- Route groups with (parentheses) for organization without affecting URL

## Directory Structure
```
app/
  (auth)/          — Auth-related routes (grouped)
    login/page.tsx
    register/page.tsx
  (dashboard)/     — Dashboard routes (grouped)
    page.tsx
    settings/page.tsx
  api/             — API routes
    route.ts
  layout.tsx       — Root layout
  page.tsx         — Home page
components/        — Shared components
  ui/              — Primitive UI components
  features/        — Feature-specific components
lib/               — Utilities, helpers, config
```

## Components
- One component per file
- PascalCase for component files and names
- Props interface defined above component
- Export as default for page components, named for shared components
```

### nextjs-data.mdc
```markdown
---
description: Next.js data fetching and caching patterns
globs: ["*.ts", "*.tsx"]
---

# Data Fetching

## Server Components (default)
- Fetch data directly in Server Components (no useEffect)
- Use async/await at the component level
- Leverage Next.js fetch caching and revalidation
- Use generateStaticParams for static generation

## Client Components
- Add 'use client' directive at top of file
- Use TanStack Query (React Query) for client-side data
- Use Server Actions for mutations (not API routes)

## Caching Strategy
- fetch() with { next: { revalidate: 3600 } } for time-based
- fetch() with { cache: 'no-store' } for dynamic data
- revalidatePath() / revalidateTag() for on-demand
- unstable_cache for database queries
```

### nextjs-styling.mdc
```markdown
---
description: Tailwind CSS and styling conventions
globs: ["*.tsx", "*.css"]
---

# Styling

## Tailwind CSS
- Use Tailwind utility classes — no custom CSS unless absolutely necessary
- Use cn() (clsx + tailwind-merge) for conditional classes
- Responsive: mobile-first (sm:, md:, lg:, xl:)
- Dark mode: dark: prefix
- Use @apply sparingly — only in component CSS modules

## Component Library
- Use shadcn/ui for pre-built components
- Customize via CSS variables in globals.css
- Extend in components/ui/ — don't modify shadcn source

## Conventions
- No inline styles — use Tailwind classes
- Extract repeated class combinations into components
- Use CSS variables for theme values (colors, spacing, fonts)
```

### nextjs-typescript.mdc
```markdown
---
description: TypeScript strict mode conventions for Next.js
globs: ["*.ts", "*.tsx"]
---

# TypeScript Strict Mode

## Config
- strict: true in tsconfig.json
- noUncheckedIndexedAccess: true
- exactOptionalPropertyTypes: true

## Patterns
- Define prop types with interface (not type) for components
- Use satisfies operator for type checking without widening
- Use const assertions for literal types
- Zod schemas for runtime validation (forms, API responses)
- Avoid type assertions (as Type) — fix the types instead
- Use discriminated unions for component variants

## Server/Client Boundary
- Server Components: async, can use await, no useState/useEffect
- Client Components: 'use client', can use hooks and browser APIs
- Pass serializable props across the boundary (no functions, classes)
```
