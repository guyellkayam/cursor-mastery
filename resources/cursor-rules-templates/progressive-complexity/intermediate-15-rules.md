# Intermediate: 15 Rules

> **TL;DR**: Covers most scenarios for mature projects. Includes the 5 starter rules plus 10 additional rules for framework conventions, testing depth, documentation, and workflow patterns.

## Includes All 5 Starter Rules

1. Project Overview (`alwaysApply: true`)
2. Code Quality (`alwaysApply: true`)
3. Git Workflow (`alwaysApply: true`)
4. Security Basics (`alwaysApply: true`)
5. Agent Instructions (`alwaysApply: true`)

See [starter-5-rules.md](starter-5-rules.md) for these.

---

## Rule 6: TypeScript Conventions

```markdown
---
description: TypeScript coding conventions
globs: ["*.ts", "*.tsx"]
---

# TypeScript Conventions

- Use strict mode (strict: true in tsconfig)
- Prefer interfaces over type aliases for object shapes
- Use type instead of interface for unions, intersections, mapped types
- Always type function parameters and return values
- Avoid `any` — use `unknown` if type is truly unknown
- Use readonly for properties that shouldn't change
- Prefer const assertions for literal types
- Use discriminated unions for state machines
- Barrel exports in index.ts for public APIs only
```

---

## Rule 7: Python Conventions

```markdown
---
description: Python coding conventions
globs: ["*.py"]
---

# Python Conventions

- Use type hints for all function signatures
- Use dataclasses or Pydantic models for data structures
- Use pathlib.Path instead of os.path
- Use f-strings for string formatting
- Use enumerate() instead of manual counter
- Use list/dict comprehensions when they're readable
- Use context managers (with) for resource management
- Follow PEP 8 naming: snake_case for functions/variables, PascalCase for classes
- Use ruff for formatting and linting
```

---

## Rule 8: Testing Depth

```markdown
---
description: Detailed testing requirements
globs: ["*test*", "*spec*"]
---

# Testing Standards

## Test Structure (AAA Pattern)
1. **Arrange** — Set up test data and dependencies
2. **Act** — Execute the code under test
3. **Assert** — Verify the result

## Coverage Requirements
- New features: minimum 80% line coverage
- Bug fixes: must include regression test
- Edge cases: null, empty, boundary values
- Error paths: verify error handling works

## Naming Convention
- test_[method]_[scenario]_[expected] (Python)
- it('should [expected] when [scenario]') (TypeScript)

## What to Test
- Business logic and calculations
- Error handling and edge cases
- API contract (request/response shape)
- Integration points (database, external APIs)

## What NOT to Test
- Third-party library internals
- Simple getters/setters with no logic
- Framework boilerplate
```

---

## Rule 9: API Design

```markdown
---
description: REST API design conventions
globs: ["**/api/**", "**/routes/**", "**/endpoints/**"]
---

# API Design Rules

- Use plural nouns for resources (/users, /orders)
- Use HTTP methods correctly (GET=read, POST=create, PUT=replace, PATCH=update, DELETE=remove)
- Return appropriate status codes (200, 201, 400, 401, 403, 404, 500)
- Use consistent error response format:
  { "error": { "code": "NOT_FOUND", "message": "User not found" } }
- Paginate list endpoints (?page=1&limit=20)
- Version APIs in URL (/api/v1/) or header
- Validate request bodies with schemas
- Document endpoints with OpenAPI/Swagger comments
```

---

## Rule 10: Database Rules

```markdown
---
description: Database interaction patterns
globs: ["**/models/**", "**/db/**", "**/migrations/**", "**/*repository*"]
---

# Database Rules

- Use migrations for ALL schema changes — never modify schema directly
- Use parameterized queries — never concatenate SQL strings
- Add indexes for frequently queried columns
- Use transactions for multi-step operations
- Handle connection errors gracefully with retries
- Use connection pooling in production
- Name migrations descriptively: 001_create_users_table.sql
- Always add created_at and updated_at timestamps
```

---

## Rule 11: Docker & Containers

```markdown
---
description: Docker and container conventions
globs: ["Dockerfile*", "docker-compose*", "*.dockerfile"]
---

# Docker Conventions

- Use multi-stage builds for smaller images
- Pin base image versions (node:20-alpine, NOT node:latest)
- Run as non-root user
- Use .dockerignore to exclude unnecessary files
- Order layers by change frequency (dependencies before source code)
- Use HEALTHCHECK for production containers
- Set resource limits in docker-compose
- Use environment variables for configuration (12-factor app)
```

---

## Rule 12: Documentation

```markdown
---
description: Documentation standards
globs: ["*.md", "**/*.md"]
---

# Documentation Standards

- README.md: What it is, how to install, how to use, how to contribute
- Keep docs next to the code they describe
- Update docs when behavior changes — stale docs are worse than no docs
- Use code examples that actually work (test them)
- Document WHY, not just WHAT (code shows what, docs explain why)
- Use Mermaid diagrams for architecture and data flow
- API docs: include request/response examples
```

---

## Rule 13: Error Handling

```markdown
---
description: Detailed error handling patterns
alwaysApply: true
---

# Error Handling Patterns

## Custom Error Classes
- Create domain-specific error classes (NotFoundError, ValidationError, AuthError)
- Include error code, message, and context
- Don't expose internal details in user-facing errors

## Logging
- Log errors with context: what happened, what was expected, what input caused it
- Use structured logging (JSON format) in production
- Include request ID for traceability
- Log at appropriate levels: ERROR for failures, WARN for degraded, INFO for operations

## Recovery
- Retry transient failures (network, database connections) with exponential backoff
- Circuit breaker for external service calls
- Graceful degradation over hard failures
```

---

## Rule 14: Performance

```markdown
---
description: Performance considerations
alwaysApply: false
---

# Performance Guidelines

- Avoid N+1 queries — use eager loading / joins
- Cache expensive computations and API calls
- Use pagination for large data sets
- Lazy-load heavy dependencies
- Profile before optimizing — don't guess at bottlenecks
- Set timeouts for external calls (database, API, HTTP)
- Use async/await for I/O-bound operations
```

---

## Rule 15: CI/CD

```markdown
---
description: CI/CD pipeline conventions
globs: [".github/workflows/**", "Jenkinsfile", "*.yaml"]
---

# CI/CD Rules

- All PRs must pass CI before merge
- Pipeline stages: lint → test → build → deploy
- Cache dependencies between runs
- Use matrix strategy for multi-version testing
- Secrets via CI/CD secrets manager — never in code
- Pin action versions (uses: actions/checkout@v4, NOT @latest)
- Fail fast — stop pipeline on first failure
- Include smoke tests in deploy stage
```

---

## Installation

```bash
mkdir -p .cursor/rules

# Copy each rule into .cursor/rules/ as individual .mdc files
# Customize the globs and content for your specific project
```

## When to Graduate to Expert

Move to the [30-rule expert set](expert-30-rules.md) when:
- You want Boris Cherny's full orchestration principles
- You need domain-specific rules (DevOps, finance, security)
- You're running multi-agent workflows with subagents
- You want advanced token optimization patterns
