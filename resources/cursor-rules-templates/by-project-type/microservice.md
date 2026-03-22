# Microservice Project Rules

> **TL;DR**: Rules for containerized API services — Docker, REST/gRPC, health checks, observability, testing.

## Rules Set (Copy to `.cursor/rules/`)

### microservice-overview.mdc
```markdown
---
description: Microservice architecture overview and conventions
alwaysApply: true
---

# Microservice Conventions

## Architecture
- Each service owns its data (no shared databases)
- Communication via REST APIs or gRPC
- Async events via message queue (Kafka, RabbitMQ, NATS)
- Service discovery via DNS or service mesh

## API Design
- RESTful endpoints with versioning (/api/v1/)
- OpenAPI/Swagger documentation for every endpoint
- Consistent error response format
- Request/response validation with schemas
- Pagination for list endpoints

## Health & Readiness
- /healthz — liveness probe (is the process running?)
- /readyz — readiness probe (can it serve traffic?)
- Include dependency checks in readiness (database, cache)
```

### docker-conventions.mdc
```markdown
---
description: Docker and container conventions for microservices
globs: ["Dockerfile*", "docker-compose*"]
---

# Docker Rules

- Multi-stage builds: builder → runtime
- Pin base image versions (node:20-alpine, python:3.12-slim)
- Non-root user in production
- HEALTHCHECK instruction
- .dockerignore excludes: .git, node_modules, __pycache__, .env
- Layer ordering: system deps → app deps → source code
- Resource limits in docker-compose
- Use BuildKit for faster builds (DOCKER_BUILDKIT=1)
```

### testing-microservice.mdc
```markdown
---
description: Testing strategy for microservices
globs: ["*test*", "*spec*", "tests/**"]
---

# Microservice Testing

## Test Pyramid
- Unit tests (70%): Business logic, utilities
- Integration tests (20%): Database, API endpoints
- E2E tests (10%): Cross-service workflows

## Contract Testing
- Define API contracts with OpenAPI
- Consumer-driven contract tests (Pact)
- Verify contracts in CI

## Test Isolation
- Use test containers for database tests
- Mock external services (not internal logic)
- Each test cleans up its own data
```

### observability.mdc
```markdown
---
description: Observability patterns for microservices
globs: ["**/monitoring/**", "**/metrics/**", "**/logging/**"]
---

# Observability

## Logging
- Structured JSON format
- Include: timestamp, level, service, request_id, message
- Correlation ID propagation across services

## Metrics
- RED method: Rate, Errors, Duration per endpoint
- Custom business metrics where relevant
- Prometheus exposition format (/metrics endpoint)

## Tracing
- OpenTelemetry for distributed tracing
- Trace context propagation in headers
- Span for each external call (DB, API, queue)
```
