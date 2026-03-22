# AGENTS.md Templates

> **TL;DR**: Ready-to-use AGENTS.md files for different project types. AGENTS.md gives AI agents project context without consuming per-request tokens (loaded once, shared).

## What Is AGENTS.md?

AGENTS.md is a project-level document that provides AI agents with:
- Architecture overview
- Code standards and conventions
- Testing requirements
- Security considerations
- Common patterns

**Key difference from .cursor/rules/**: AGENTS.md is descriptive (explains the project) while rules are prescriptive (tells the agent what to do).

## Placement

```
project-root/
  AGENTS.md              ← Project-wide context
  src/
    auth/
      AGENTS.md          ← Auth subsystem context
    api/
      AGENTS.md          ← API subsystem context
```

Subsystem AGENTS.md files provide focused context when the agent works in that directory.

---

## Template: Microservice

```markdown
# AGENTS.md — [Service Name]

## Architecture
This is a [language] microservice that [purpose].
It communicates with [other services] via [REST/gRPC/events].

## Tech Stack
- Runtime: [Node.js 20 / Python 3.12 / Go 1.22]
- Framework: [Express / FastAPI / Gin]
- Database: [PostgreSQL 16 via Prisma / SQLAlchemy / GORM]
- Cache: [Redis 7]
- Message Queue: [Kafka / RabbitMQ / NATS]

## Directory Structure
```
src/
  routes/      — API endpoint handlers
  services/    — Business logic
  models/      — Database models and types
  middleware/  — Auth, logging, error handling
  utils/       — Shared utilities
tests/
  unit/        — Unit tests (mock dependencies)
  integration/ — Integration tests (real DB)
```

## Code Standards
- TypeScript strict mode
- Functional style preferred (pure functions, immutability)
- Error handling: custom error classes, never silent catches
- Logging: structured JSON via winston/pino

## Testing
- Jest with ts-jest transformer
- Test files: *.test.ts alongside source
- Integration tests use test containers
- Minimum 80% coverage for new code

## API Conventions
- RESTful with /api/v1/ prefix
- Consistent error format: { error: { code, message } }
- Pagination: ?page=1&limit=20
- Auth: Bearer JWT in Authorization header

## Database
- Migrations via Prisma migrate
- Seed data in prisma/seed.ts
- Never modify schema directly — always via migration
```

---

## Template: Terraform Infrastructure

```markdown
# AGENTS.md — Infrastructure

## Architecture
Infrastructure managed via Terraform with GitOps deployment via ArgoCD.
Cloud: AWS with multi-AZ setup.

## Stack
- Terraform 1.7+
- AWS provider (pinned version)
- S3 + DynamoDB for remote state
- GitHub Actions for CI/CD

## Module Structure
```
terraform/
  modules/
    networking/    — VPC, subnets, security groups
    compute/       — EC2, ECS, Lambda
    database/      — RDS, ElastiCache
    monitoring/    — CloudWatch, alerts
  environments/
    dev/           — Development account
    staging/       — Staging account
    prod/          — Production account (requires approval)
```

## Conventions
- All resources tagged: environment, team, project, managed-by
- Variables: description + type + validation required
- Outputs: for cross-module references
- No hardcoded IDs — use data sources
- terraform fmt before every commit

## Security
- tfsec/checkov scanning in CI
- No secrets in .tf files
- IAM least privilege
- Encryption at rest for all storage
- VPC flow logs enabled

## Deployment
- Plan → Review → Apply (never auto-apply to prod)
- State locking via DynamoDB
- Workspace per environment
```

---

## Template: Python Data Project

```markdown
# AGENTS.md — Data Project

## Architecture
Data pipeline processing [data source] into [output format/destination].

## Stack
- Python 3.12
- pandas / polars for data processing
- SQLAlchemy for database access
- pytest for testing
- ruff for linting/formatting

## Directory Structure
```
src/
  pipelines/     — ETL pipeline definitions
  models/        — Pydantic data models
  transforms/    — Data transformation functions
  connectors/    — Source/destination adapters
  utils/         — Shared utilities
tests/
  fixtures/      — Test data files
  unit/          — Unit tests
  integration/   — Integration tests
notebooks/       — Exploration and analysis
```

## Conventions
- Type hints on all function signatures
- Pydantic models for data validation
- Idempotent pipeline operations
- Incremental processing preferred over full reloads
- Dead letter queue for failed records

## Testing
- pytest with fixtures
- Use small representative datasets (not prod data)
- Test transforms independently
- Integration tests with test database
- Data quality assertions in pipeline code

## Data Quality
- Schema validation on input AND output
- Null checks, duplicate detection
- Row count assertions
- Freshness checks (data not stale)
```

---

## Template: Next.js Frontend

```markdown
# AGENTS.md — Frontend

## Architecture
Next.js App Router application with React Server Components.

## Stack
- Next.js 15 (App Router)
- React 19
- TypeScript 5 (strict mode)
- Tailwind CSS 4
- shadcn/ui components
- TanStack Query for client data

## Directory Structure
```
app/
  (auth)/        — Authentication routes
  (dashboard)/   — Dashboard routes
  api/           — API routes
  layout.tsx     — Root layout
components/
  ui/            — shadcn/ui primitives
  features/      — Feature components
lib/
  api/           — API client functions
  utils/         — Utility functions
  hooks/         — Custom React hooks
```

## Conventions
- Server Components by default — 'use client' only when needed
- Server Actions for mutations (not API routes)
- Zod for form validation
- cn() for conditional Tailwind classes
- One component per file, PascalCase

## Testing
- Vitest for unit tests
- Playwright for E2E tests
- React Testing Library for component tests
- MSW for API mocking

## Styling
- Tailwind utility classes only (no custom CSS)
- Mobile-first responsive design
- Dark mode via CSS variables
- shadcn/ui as component foundation
```

---

## "Use this when..."

| Scenario | Template |
|----------|----------|
| New Node.js/Python microservice | Microservice template |
| Infrastructure with Terraform | Terraform template |
| Data pipeline or analytics project | Python Data template |
| Frontend with Next.js/React | Next.js template |
| Starting any new project | Pick closest template, customize |
