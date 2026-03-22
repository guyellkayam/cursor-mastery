# Database MCP Servers & Rules

> **TL;DR**: MCP servers for querying databases from Cursor, plus .mdc rules for database conventions.

## MCP Servers

### PostgreSQL MCP
```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres", "postgresql://user:pass@localhost:5432/dbname"],
      "env": {}
    }
  }
}
```
**Tools**: Run queries, list tables, describe schema, read data.

### Redis MCP
```json
{
  "mcpServers": {
    "redis": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-redis"],
      "env": { "REDIS_URL": "redis://localhost:6379" }
    }
  }
}
```

### PlanetScale Plugin
**Install**: `/add-plugin planetscale`
- Manage MySQL-compatible databases, branches, deploy requests.

### Hasura MCP
```json
{
  "mcpServers": {
    "hasura": {
      "command": "npx",
      "args": ["-y", "@hasura/mcp-server"],
      "env": { "HASURA_GRAPHQL_ENDPOINT": "...", "HASURA_ADMIN_SECRET": "..." }
    }
  }
}
```

## Database Rules

### database.mdc
```markdown
---
description: Database interaction conventions
globs: ["**/models/**", "**/db/**", "**/migrations/**", "*.sql"]
---

# Database Rules

- Migrations for ALL schema changes (never modify directly)
- Parameterized queries (never concatenate SQL strings)
- Indexes on frequently queried columns
- Transactions for multi-step operations
- Connection pooling in production
- Descriptive migration names: 001_create_users_table
- created_at and updated_at timestamps on all tables
- Soft delete (deleted_at) where business requires it
- Explain plans for complex queries
```

## "Use this when..."
| Scenario | Tool |
|----------|------|
| Querying PostgreSQL from Cursor | PostgreSQL MCP |
| Cache management | Redis MCP |
| MySQL with branching workflow | PlanetScale plugin |
| GraphQL API over database | Hasura MCP |
