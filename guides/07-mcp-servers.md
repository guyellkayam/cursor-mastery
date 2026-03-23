# Guide 07: MCP Servers in Cursor

> **Level**: 5-6
> **Prerequisites**: Guides 01-03
> **Official Docs**: https://cursor.com/docs/context/mcp

## What Is MCP?

MCP (Model Context Protocol) lets Cursor's AI connect to external tools and data sources. Think of MCP servers as plugins that give the AI new abilities: query databases, manage GitHub issues, search documentation, interact with APIs.

**How it works**: MCP servers expose "tools" that the agent can call. When you ask the agent to "check the database" or "create a GitHub issue", it uses MCP tools to do it.

## Configuration

### Where Config Lives

| File | Scope |
|------|-------|
| `.cursor/mcp.json` | Project-specific (committed to git) |
| `~/.cursor/mcp.json` | Global (all projects) |

### File Format

```json
{
  "mcpServers": {
    "server-name": {
      "command": "executable",
      "args": ["arg1", "arg2"],
      "env": {
        "API_KEY": "your-key"
      }
    }
  }
}
```

### Transport Types

| Transport | Use Case | Config |
|-----------|----------|--------|
| **stdio** | Local servers (most common) | `command` + `args` |
| **sse** | Remote servers | URL-based |
| **streamable-http** | Remote with streaming | URL-based |

## Setup Methods

### Method 1: UI (Easiest)
1. Open Settings (`Cmd+,`)
2. Go to **Tools & MCP** (or **Features > MCP**)
3. Click **+ Add New MCP Server**
4. Enter name, transport, command/URL
5. Save — green dot = connected

### Method 2: Config File
Create `.cursor/mcp.json`:

```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${env:GITHUB_TOKEN}"
      }
    }
  }
}
```

### Method 3: One-Click Install
Some servers offer deep links: `cursor://install-mcp/...`
These handle OAuth and configuration automatically.

## Popular MCP Servers

> As of March 2026, there are 10,000+ public MCP servers. The top servers by usage (per FastMCP analytics) are Context7, Playwright, and Desktop Commander.

### Development & Git

**GitHub**
```json
{
  "mcpServers": {
    "github": {
      "command": "docker",
      "args": [
        "run", "-i", "--rm",
        "-e", "GITHUB_PERSONAL_ACCESS_TOKEN",
        "ghcr.io/github/github-mcp-server"
      ],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "${env:GITHUB_TOKEN}"
      }
    }
  }
}
```

**Filesystem**
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/allowed/dir"]
    }
  }
}
```

### Databases

**PostgreSQL**
```json
{
  "mcpServers": {
    "postgres": {
      "command": "npx",
      "args": ["-y", "mcp-server-postgres"],
      "env": {
        "DATABASE_URL": "${env:DATABASE_URL}"
      }
    }
  }
}
```

**SQLite**
```json
{
  "mcpServers": {
    "sqlite": {
      "command": "python",
      "args": ["-m", "mcp_server_sqlite", "/path/to/database.db"]
    }
  }
}
```

### Browser Automation

**Playwright** (Top 2 most popular MCP server)
```json
{
  "mcpServers": {
    "playwright": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-server-playwright"]
    }
  }
}
```

### External Services

**Google Services** (Official MCP — announced 2026)
```json
{
  "mcpServers": {
    "google": {
      "command": "npx",
      "args": ["mcp-remote", "https://mcp.googleapis.com/sse"]
    }
  }
}
```

**Asana (Remote SSE)**
```json
{
  "mcpServers": {
    "asana": {
      "command": "npx",
      "args": ["mcp-remote", "https://mcp.asana.com/sse"]
    }
  }
}
```

### Memory & Context

**Context7** (Most popular MCP server — 11K+ views on FastMCP)
```json
{
  "mcpServers": {
    "context7": {
      "command": "npx",
      "args": ["-y", "context7-mcp"]
    }
  }
}
```

## Advanced: Profiles

Switch MCP configs based on environment (dev, staging, prod):

```json
{
  "profiles": {
    "development": {
      "mcpServers": {
        "database": {
          "command": "mcp-server-postgres",
          "env": { "DATABASE_URL": "${env:DEV_DB_URL}" }
        }
      }
    },
    "staging": {
      "mcpServers": {
        "database": {
          "command": "mcp-server-postgres",
          "env": { "DATABASE_URL": "${env:STAGING_DB_URL}" }
        }
      }
    }
  },
  "profileSwitcher": {
    "automatic": true,
    "detection": "git-branch",
    "mappings": {
      "develop": "development",
      "staging": "staging"
    }
  }
}
```

## Using MCP in Agent Mode

Once configured, the agent auto-discovers MCP tools. Test with:
```
What tools do you have access to?
```

Then use naturally:
```
Check the database for users created in the last 24 hours.
```

```
Create a GitHub issue titled "Fix login redirect" with
label "bug" in the frontend repo.
```

## Best Practices

### Do
- Use project `.cursor/mcp.json` for project-specific tools (DB, APIs)
- Use global `~/.cursor/mcp.json` for universal tools (GitHub, filesystem)
- Store secrets in environment variables, reference with `${env:VAR_NAME}`
- Limit active servers to what you actually need (context budget)
- Restart Cursor or click Reload after config changes

### Don't
- Hardcode API keys in mcp.json
- Enable production database access in development
- Add too many servers (each adds tool descriptions to context)
- Commit secrets to git (use .env + ${env:} references)

## Troubleshooting

| Issue | Fix |
|-------|-----|
| Server not connecting | Check green dot in Settings > MCP. Restart Cursor. |
| "No tools available" | Verify mcp.json syntax. Check server process is running. |
| Permission errors | Check env vars are set. Verify API keys. |
| Slow responses | Too many servers. Disable unused ones. |

## MCP Ecosystem (2026)

The MCP ecosystem matured significantly in 2025-2026:

- **Governance**: Anthropic donated MCP to the Agentic AI Foundation (AAIF) under the Linux Foundation (Dec 2025), co-founded by Anthropic, Block, and OpenAI
- **SDK**: TypeScript SDK v1.27 (Feb 2026) — 34,700+ dependent projects on npm
- **Scale**: 10,000+ public servers, MCP.so marketplace, FastMCP Python framework
- **Google**: Official managed MCP servers for all Google/Google Cloud services, plus Colab MCP Server
- **2026 Roadmap Priorities**: Transport & server discovery (`.well-known`), agentic lifecycle (Tasks primitive), enterprise readiness (audit trails, SSO, gateways), governance maturation

### MCP Apps (Cursor 2.6)

MCP servers can now render interactive UIs directly inside Cursor chats — charts from Amplitude, diagrams from Figma, whiteboards from tldraw. This is the "MCP Apps" feature introduced in Cursor 2.6 (March 3, 2026).

## Sources
- [Cursor Docs: MCP](https://cursor.com/docs/context/mcp)
- [MCP 2026 Roadmap](https://modelcontextprotocol.io/development/roadmap)
- [FastMCP: Top MCP Servers 2026](https://fastmcp.me/blog/top-10-most-popular-mcp-servers)
- [Google Cloud: MCP Support](https://cloud.google.com/blog/products/ai-machine-learning/announcing-official-mcp-support-for-google-services)
