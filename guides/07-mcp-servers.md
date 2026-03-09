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

### External Services

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

**Puppeteer (Browser Automation)**
```json
{
  "mcpServers": {
    "puppeteer": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-puppeteer"]
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

## Sources
- [Cursor Docs: MCP](https://cursor.com/docs/context/mcp)
- [TrueFoundry: MCP in Cursor](https://www.truefoundry.com/blog/mcp-servers-in-cursor-setup-configuration-and-security-guide)
- [Fast.io: Cursor MCP Setup](https://fast.io/resources/cursor-mcp-server-setup/)
