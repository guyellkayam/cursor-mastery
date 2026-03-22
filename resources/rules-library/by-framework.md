# Rules by Framework

> **TL;DR**: Community-curated .mdc rules organized by framework.

## Next.js (App Router)
**Source**: blefnk/awesome-cursor-rules, cursor.directory
- Server Components by default, 'use client' only when needed
- App Router with route groups
- Server Actions for mutations
- Tailwind CSS + shadcn/ui
- TanStack Query for client data
- Zod for validation

## React
- Functional components only
- Custom hooks for reusable logic
- Composition over prop drilling
- React.memo for performance-critical renders
- Error boundaries for fault tolerance

## FastAPI
- Pydantic schemas for all I/O
- Dependency injection via Depends()
- Async endpoints for I/O-bound
- Middleware for cross-cutting concerns
- TestClient for testing

## Django
- Fat models, thin views
- Class-based views for CRUD
- Custom managers for queries
- Migrations: one per PR
- django-environ for config

## Express / Node.js
- Middleware chain architecture
- Error handling middleware (last in chain)
- Validation with Joi/Zod
- Structured logging (winston/pino)
- Graceful shutdown handling

## Angular
- Smart/Dumb component pattern
- RxJS for state management
- Lazy loading for modules
- OnPush change detection
- NgRx for complex state

## Astro
- Island architecture
- Static-first, hydrate when needed
- Content collections for structured content
- View Transitions API
- Framework-agnostic components

## Where to Find More

Browse the full list at [awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules) — organized by framework with install instructions.
