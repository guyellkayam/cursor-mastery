# Python Workflows for Cursor

> **TL;DR**: Copy-paste Cursor agent workflows for Python development — FastAPI setup, pytest TDD, data science exploration, and dependency management.

## Workflow 1: FastAPI Project Setup

```
Plan Mode (Shift+Tab):
"Create a FastAPI project with:
- Async PostgreSQL via SQLAlchemy 2.0 + asyncpg
- Pydantic v2 schemas
- JWT authentication
- Docker Compose for local development
- pytest with async test client
- Alembic for migrations

Follow the structure from @file:resources/python/rules.md (FastAPI section)
Use pyproject.toml for dependencies (not requirements.txt)"
```

## Workflow 2: TDD with pytest

```
Step 1: "Write a failing test for [feature]:
- Test the happy path
- Test validation errors
- Test edge cases (empty, null, boundary)
Use pytest fixtures from conftest.py. @file:tests/conftest.py"

Step 2: "Run the tests — confirm they fail"

Step 3: "Implement the minimum code to make all tests pass.
Follow existing patterns from @file:src/services/user.py"

Step 4: "Refactor for clarity while keeping all tests green.
Run pytest --cov to check coverage."
```

## Workflow 3: Data Science Exploration

```
"Create a Jupyter notebook for analyzing [dataset]:

1. Load and inspect data (shape, dtypes, head, describe)
2. Data quality checks (nulls, duplicates, outliers)
3. Exploratory visualization (distributions, correlations, trends)
4. Key findings summary in markdown cells

Use pandas for processing, plotly for interactive charts.
Set random seed 42 for reproducibility.
Add markdown headers between each section."
```

## Workflow 4: Dependency Management

```
"Audit project dependencies:
1. Run 'pip list --outdated' to find stale packages
2. Check for security vulnerabilities: 'pip-audit'
3. Update minor versions that are safe
4. Pin all versions in pyproject.toml
5. Run full test suite to verify nothing broke"
```

## Workflow 5: API Documentation

```
"Generate OpenAPI documentation for the FastAPI app:
1. Review all routers and endpoints
2. Ensure every endpoint has:
   - Summary and description
   - Request/response Pydantic models
   - Example values
   - Error responses (400, 401, 404, 500)
3. Add tags for grouping related endpoints
4. Verify at /docs (Swagger UI) and /redoc"
```

## Workflow 6: Database Migration

```
"Create a database migration for [change]:
1. Modify the SQLAlchemy model
2. Generate migration: alembic revision --autogenerate -m '[description]'
3. Review the generated migration script
4. Test: alembic upgrade head (on dev database)
5. Verify schema matches model
6. Add rollback: alembic downgrade -1 (verify it works)"
```
