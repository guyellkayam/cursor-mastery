# Python Data Pipeline Rules

> **TL;DR**: Rules for ETL/ELT data pipelines — schema validation, idempotency, error handling, data quality checks.

## Rules Set (Copy to `.cursor/rules/`)

### pipeline-overview.mdc
```markdown
---
description: Data pipeline architecture and conventions
globs: ["*.py", "pipelines/**", "dags/**", "etl/**"]
---

# Data Pipeline Conventions

## Architecture Principles
- Idempotent operations — running twice produces the same result
- Schema-on-write — validate data before writing
- Incremental processing — avoid full reloads when possible
- Immutable data — append/version, don't update in place
- Separation of concerns: extract, transform, load in separate functions

## Pipeline Structure
```
pipelines/
  pipeline_name/
    __init__.py
    extract.py      — Data extraction from sources
    transform.py    — Business logic and transformations
    load.py         — Write to destination
    schemas.py      — Pydantic models for validation
    config.py       — Pipeline configuration
    tests/          — Pipeline-specific tests
```

## Data Quality
- Validate schema on input AND output
- Check for nulls, duplicates, and out-of-range values
- Row count assertions (expected vs actual)
- Data freshness checks (timestamp not older than threshold)
```

### pipeline-python.mdc
```markdown
---
description: Python conventions for data pipelines
globs: ["*.py"]
---

# Python Data Pipeline Standards

## Type Hints
- All function signatures must have type hints
- Use Pydantic models for data validation
- Use TypedDict for dictionary structures

## Data Handling
- Use pandas DataFrames for tabular data processing
- Use polars for large datasets (faster than pandas)
- Use pathlib.Path for file paths
- Use context managers for file/DB connections
- Stream large files — don't load entire dataset into memory

## Error Handling
- Wrap each pipeline stage in try/except
- Log failed records with context (row number, input values)
- Dead letter queue for failed records
- Retry transient failures with exponential backoff
- Never silently drop data

## Configuration
- Use environment variables for credentials
- Use config files (YAML/TOML) for pipeline parameters
- Different configs per environment (dev, staging, prod)
```

### pipeline-testing.mdc
```markdown
---
description: Testing data pipelines
globs: ["*test*", "tests/**"]
---

# Pipeline Testing

## Unit Tests
- Test each transform function independently
- Use fixture data (small, representative datasets)
- Test edge cases: empty input, single row, nulls, duplicates
- Test schema validation catches bad data

## Integration Tests
- Test full extract → transform → load flow
- Use test database or temp files as destinations
- Verify output matches expected schema and row counts
- Test with realistic data volumes (at least 1000 rows)

## Data Quality Tests
- Assertion tests: not_null, unique, accepted_values
- Freshness tests: data not older than threshold
- Relationship tests: referential integrity between tables
- Volume tests: row counts within expected range
```

### pipeline-scheduling.mdc
```markdown
---
description: Pipeline scheduling and orchestration conventions
globs: ["dags/**", "airflow/**", "*scheduler*"]
---

# Pipeline Scheduling

## Conventions
- Use cron expressions for scheduling
- All times in UTC
- Document the schedule in pipeline README
- Set SLA alerts for late pipelines
- Use dependency-based triggers (not just time-based)

## Retry Policy
- Max 3 retries for transient failures
- Exponential backoff between retries
- Alert on final failure (not on each retry)
- Separate retry logic from business logic

## Monitoring
- Track: execution time, rows processed, success/failure
- Alert on: failures, SLA breaches, data quality violations
- Dashboard: pipeline health overview, recent runs, data freshness
```
