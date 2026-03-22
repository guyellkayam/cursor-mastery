# Data MCP Servers

> **TL;DR**: MCP servers for databases and data APIs — PostgreSQL, Redis, AlphaVantage, Hasura, Brave Search.

## PostgreSQL MCP
```json
{ "postgres": { "command": "npx", "args": ["-y", "@modelcontextprotocol/server-postgres", "postgresql://user:pass@localhost:5432/db"] } }
```
**Tools**: Run queries, list tables, describe schema.

## Redis MCP
```json
{ "redis": { "command": "npx", "args": ["-y", "@modelcontextprotocol/server-redis"], "env": { "REDIS_URL": "redis://localhost:6379" } } }
```
**Tools**: Get/set keys, list keys, manage data structures.

## AlphaVantage MCP
```json
{ "alphavantage": { "command": "npx", "args": ["-y", "@anthropic/mcp-alphavantage"], "env": { "ALPHAVANTAGE_API_KEY": "..." } } }
```
**Tools**: Stock quotes, time series, fundamentals, forex, crypto.

## Hasura GraphQL MCP
```json
{ "hasura": { "command": "npx", "args": ["-y", "@hasura/mcp-server"], "env": { "HASURA_GRAPHQL_ENDPOINT": "...", "HASURA_ADMIN_SECRET": "..." } } }
```
**Tools**: GraphQL queries, schema introspection, mutation execution.

## Brave Search MCP
```json
{ "brave-search": { "command": "npx", "args": ["-y", "@modelcontextprotocol/server-brave-search"], "env": { "BRAVE_API_KEY": "..." } } }
```
**Tools**: Web search, news search — useful for researching current data.

## Firecrawl MCP (URL to Markdown)
```json
{ "firecrawl": { "command": "npx", "args": ["-y", "firecrawl-mcp"], "env": { "FIRECRAWL_API_KEY": "..." } } }
```
**Tools**: Convert any URL to clean Markdown for AI context. Preserves document structure.

## Apify MCP (3,000+ Web Scrapers)
```json
{ "apify": { "command": "npx", "args": ["-y", "@apify/actors-mcp-server"], "env": { "APIFY_TOKEN": "..." } } }
```
**Tools**: 3,000+ pre-built web scraping actors — Google Maps, LinkedIn, Amazon, social media data.

## Memory MCP (Knowledge Graph)
```json
{ "memory": { "command": "npx", "args": ["-y", "@modelcontextprotocol/server-memory"] } }
```
**Tools**: Persistent memory via knowledge graph. Retains context across sessions.
