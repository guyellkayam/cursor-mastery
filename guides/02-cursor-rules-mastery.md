# Guide 02: Cursor Rules Mastery

> **Level**: 2-3
> **Prerequisites**: Guide 01
> **Official Docs**: https://cursor.com/docs/context/rules

## Why Rules Matter

Rules are Cursor's equivalent of Claude Code's CLAUDE.md. They inject persistent instructions into every AI interaction, ensuring consistent code style, architecture patterns, and project conventions. Without rules, the AI guesses your preferences every time.

**The key insight**: Rules reduce prompt repetition. Instead of saying "use TypeScript strict mode, functional components, and Tailwind" in every prompt, encode it once in rules.

## The Two Systems

### Legacy: `.cursorrules` (Root File)
A single markdown file in your project root. Still supported but limited.

```markdown
# .cursorrules
You are an expert TypeScript developer working on a Next.js 14 app.

## Stack
- Next.js 14 with App Router
- TypeScript strict mode
- Tailwind CSS
- Prisma ORM with PostgreSQL

## Conventions
- Use functional components with hooks
- PascalCase for components, camelCase for utilities
- Always add JSDoc for exported functions
- Prefer server components by default
```

### Modern: `.cursor/rules/` Directory (MDC Files)
The recommended approach. Create multiple `.mdc` (Markdown Configuration) files, each scoped to specific file patterns.

```
.cursor/
  rules/
    project-overview.mdc      # Always applied
    typescript.mdc             # *.ts, *.tsx files
    api-routes.mdc             # app/api/**
    components.mdc             # components/**
    database.mdc               # prisma/**, *.sql
    testing.mdc                # **/*.test.*, **/*.spec.*
```

## MDC File Format

Every `.mdc` file has YAML-like frontmatter + Markdown content:

```markdown
---
description: TypeScript strict conventions for all TS/TSX files
globs: **/*.ts, **/*.tsx
alwaysApply: false
---

# TypeScript Conventions

## Type Safety
- Enable strict mode in tsconfig.json
- Never use `any` — use `unknown` with type guards
- Prefer interfaces over types for object shapes
- Use discriminated unions for variant types

## Naming
- Interfaces: `IUserService` or `UserService` (no prefix)
- Types: `UserRole`, `ApiResponse<T>`
- Enums: `PascalCase` with `PascalCase` members

## Imports
- Use absolute imports from `@/` prefix
- Group: external libs > internal modules > relative
- Never use default exports (except pages/layouts)
```

### Frontmatter Fields

| Field | Type | Description |
|-------|------|-------------|
| `description` | string | Agent-readable summary (< 120 chars). Format: "ACTION when TRIGGER to OUTCOME" |
| `globs` | string | Comma-separated glob patterns: `**/*.ts, **/*.tsx` |
| `alwaysApply` | boolean | `true` = loaded in every AI interaction (use sparingly) |

### Rule Types (How They Activate)

| Type | Trigger | Frontmatter |
|------|---------|-------------|
| **Always** | Every interaction | `alwaysApply: true` |
| **Auto Attached** | File matches `globs` | `globs: **/*.ts` |
| **Agent Requested** | AI picks based on `description` | Good description, no globs |
| **Manual** | User types `@rule-name` | Any config |

## Real-World Rule Examples

### Project Overview (Always Applied)
```markdown
---
description: Core project context and conventions
globs:
alwaysApply: true
---

# Project: E-Commerce Platform

## Tech Stack
- Next.js 14 (App Router)
- TypeScript 5.3+ (strict)
- Tailwind CSS 3.4
- Prisma 5 + PostgreSQL
- NextAuth.js v5

## Architecture
- `/app` — Routes and pages (App Router)
- `/components` — Reusable UI components
- `/lib` — Business logic and utilities
- `/prisma` — Schema and migrations

## Commands
- `bun dev` — Start development
- `bun test` — Run tests
- `bun lint` — Lint and format
```

### API Routes
```markdown
---
description: REST API route conventions for Next.js API handlers
globs: app/api/**/*.ts
alwaysApply: false
---

# API Route Conventions

## Structure
- Each route exports named HTTP method handlers: GET, POST, PUT, DELETE
- Use NextRequest/NextResponse from 'next/server'
- Validate input with Zod schemas

## Error Handling
- Return proper HTTP status codes
- Use consistent error response shape:
  ```json
  { "error": { "code": "NOT_FOUND", "message": "User not found" } }
  ```
- Never expose internal errors to clients

## Authentication
- Check session in every protected route
- Use `auth()` from NextAuth, not manual token parsing
```

### Testing Rules
```markdown
---
description: Testing patterns and conventions for test files
globs: **/*.test.ts, **/*.test.tsx, **/*.spec.ts
alwaysApply: false
---

# Testing Conventions

## Framework
- Vitest for unit/integration tests
- Playwright for E2E tests
- Testing Library for component tests

## Patterns
- Arrange-Act-Assert structure
- One assertion per test (prefer)
- Use descriptive test names: "should return 404 when user not found"
- Mock external APIs, never mock internal logic

## File Organization
- Co-locate tests: `Button.tsx` → `Button.test.tsx`
- Shared fixtures in `__fixtures__/`
- Test utilities in `test/helpers.ts`
```

## Best Practices

### Do
- Keep rules under 500 lines total (token budget)
- Use specific globs to avoid over-injection
- Write `description` as actionable sentences
- Include concrete code examples in rules
- Reference actual files with `@file:path/to/example.ts`
- Create rules reactively (after AI makes repeated mistakes)

### Don't
- Copy entire style guides into rules
- Use `alwaysApply: true` for everything
- Create overlapping rules with conflicting advice
- Write vague descriptions like "general coding"
- Forget that rules cost tokens — every rule eats context

### Rule of Thumb
Start with 2-3 rules. Add new ones only when you notice the AI repeatedly making the same mistake. Quality over quantity.

## Creating Rules

### Via Command Palette
1. `Cmd+Shift+P` > "New Cursor Rule"
2. Name the file (e.g., `react-components.mdc`)
3. Add frontmatter + content

### Via Terminal
```bash
mkdir -p .cursor/rules
cat > .cursor/rules/my-rule.mdc << 'EOF'
---
description: My custom rule description
globs: **/*.ts
alwaysApply: false
---

# Rule content here
EOF
```

### Referencing Rules Manually
In chat or composer, type `@rule-name` to explicitly inject a rule.

## Popular Community Rules

Browse community-contributed rules at:
- **cursor.directory** (73k+ members) — Browse, generate, share rules
- **awesome-cursor-rules-mdc** — Curated MDC file collection
- **digitalchild/cursor-best-practices** — Battle-tested configurations

## Sources
- [Cursor Docs: Rules](https://cursor.com/docs/context/rules)
- [Deep Dive into Cursor Rules (Forum)](https://forum.cursor.com/t/a-deep-dive-into-cursor-rules-0-45/60721)
- [cursor.directory](https://cursor.directory)
- [learn-cursor.com: Rules Guide](https://learn-cursor.com/en/blog/posts/cursor-rules-guide)
