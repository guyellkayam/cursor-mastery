# Python Rules for Cursor

> **TL;DR**: .mdc rules for Python development — style, FastAPI, Django, data science, and testing conventions.

## General Python Rules

### python-style.mdc
```markdown
---
description: Python coding style and conventions
globs: ["*.py"]
---

# Python Style

- Type hints for all function signatures and return values
- Use dataclasses or Pydantic models for data structures
- Use pathlib.Path instead of os.path
- Use f-strings for formatting
- Use enumerate() instead of manual counters
- Use list/dict comprehensions when readable
- Use context managers (with) for resources
- PEP 8: snake_case functions/variables, PascalCase classes
- Maximum line length: 88 (Black/Ruff default)
- Docstrings: Google style for public functions
- Imports: stdlib → third-party → local (blank line between groups)
```

## FastAPI Rules

### fastapi.mdc
```markdown
---
description: FastAPI project conventions
globs: ["*.py", "app/**", "api/**"]
---

# FastAPI Conventions

## Endpoint Structure
- Router per domain: app/routers/users.py, app/routers/orders.py
- Dependencies via Depends() — never instantiate in endpoint
- Pydantic models for request/response schemas
- Status codes: 200 (GET), 201 (POST), 204 (DELETE)
- HTTPException with detail for errors

## Patterns
- Async endpoints for I/O-bound operations
- Background tasks via BackgroundTasks
- Middleware for cross-cutting concerns (auth, logging, CORS)
- Lifespan events for startup/shutdown (database pool, cache)

## Testing
- TestClient for endpoint tests
- Override dependencies in tests (Depends override)
- Use httpx.AsyncClient for async tests
- Fixture for test database with rollback

## Project Structure
```
app/
  main.py          — FastAPI app creation, middleware, routers
  config.py        — Settings via pydantic-settings
  routers/         — API endpoint handlers
  models/          — SQLAlchemy/Tortoise models
  schemas/         — Pydantic request/response schemas
  services/        — Business logic
  dependencies/    — Reusable Depends() functions
  middleware/      — Custom middleware
tests/
  conftest.py      — Shared fixtures
  test_*.py        — Test files
```
```

## Django Rules

### django.mdc
```markdown
---
description: Django project conventions
globs: ["*.py", "*/models.py", "*/views.py", "*/urls.py"]
---

# Django Conventions

- Fat models, thin views — business logic in models/managers
- Use class-based views for CRUD, function-based for custom logic
- Custom managers for complex querysets
- Signals sparingly — prefer explicit method calls
- Migrations: one per PR, squash periodically
- Settings: split into base.py, development.py, production.py
- Use django-environ for environment variables
- Template tags/filters for presentation logic only
```

## Data Science Rules

### data-science.mdc
```markdown
---
description: Data science and analysis conventions
globs: ["*.py", "*.ipynb", "notebooks/**"]
---

# Data Science Conventions

- Reproducible: pin versions, set random seeds, document data sources
- Notebooks: clear outputs before commit, one analysis per notebook
- Extract reusable code into .py modules
- Use pandas for tabular data, polars for large datasets
- Visualization: title + axis labels + units on every chart
- Statistical tests: state hypothesis, significance level, sample size
- Feature engineering: document each feature's logic and source
- Model evaluation: train/val/test split, cross-validation, multiple metrics
```

## Testing Rules

### pytest.mdc
```markdown
---
description: Python testing conventions with pytest
globs: ["test_*.py", "*_test.py", "tests/**", "conftest.py"]
---

# Pytest Conventions

- File naming: test_[module].py
- Function naming: test_[what]_[scenario]_[expected]
- Use fixtures for setup/teardown (conftest.py)
- Parametrize for multiple input/output cases
- Use markers: @pytest.mark.slow, @pytest.mark.integration
- Mock external services, not internal logic
- Assert one thing per test
- Use pytest-cov for coverage: pytest --cov=app --cov-report=term-missing
- Fixture scope: function (default) for isolation, session for expensive setup
```

---

## "Use this when..."

| Scenario | Rule |
|----------|------|
| Any Python project | python-style.mdc |
| API development | fastapi.mdc or django.mdc |
| Data analysis / ML | data-science.mdc |
| Writing tests | pytest.mdc |

## Quick Install

```bash
mkdir -p .cursor/rules
# Copy the .mdc blocks above into individual files:
# .cursor/rules/python-style.mdc
# .cursor/rules/fastapi.mdc
# etc.
```
