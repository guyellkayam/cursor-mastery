# MCP Configuration Templates

> **TL;DR**: Copy-paste `.cursor/mcp.json` configs for common stacks. Choose one template, customize, and save.

## DevOps Stack (GitHub + Kubernetes)
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_YOUR_TOKEN"
      }
    },
    "kubernetes": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-kubernetes"],
      "env": {
        "KUBECONFIG": "~/.kube/config"
      }
    }
  }
}
```
**Tool count**: ~25 (well under 40 limit)

## Full-Stack Web (GitHub + PostgreSQL + Brave Search)
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_YOUR_TOKEN"
      }
    },
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres", "postgresql://user:pass@localhost:5432/mydb"]
    },
    "brave-search": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-brave-search"],
      "env": {
        "BRAVE_API_KEY": "YOUR_KEY"
      }
    }
  }
}
```
**Tool count**: ~30

## Project Management (Atlassian)
```json
{
  "mcpServers": {
    "atlassian": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-atlassian"],
      "env": {
        "JIRA_URL": "https://your-org.atlassian.net",
        "JIRA_EMAIL": "your@email.com",
        "JIRA_API_TOKEN": "YOUR_TOKEN",
        "CONFLUENCE_URL": "https://your-org.atlassian.net/wiki"
      }
    }
  }
}
```
**Tool count**: ~50 (near or at limit — use alone)

## Finance (AlphaVantage + PostgreSQL)
```json
{
  "mcpServers": {
    "alphavantage": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-alphavantage"],
      "env": {
        "ALPHAVANTAGE_API_KEY": "YOUR_KEY"
      }
    },
    "postgres": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-postgres", "postgresql://user:pass@localhost:5432/finance"]
    }
  }
}
```
**Tool count**: ~20

## Minimal (GitHub only)
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_YOUR_TOKEN"
      }
    }
  }
}
```
**Tool count**: ~15 (maximum headroom for agent performance)

## Setup Instructions

1. Create the file: `.cursor/mcp.json` (project) or `~/.cursor/mcp.json` (global)
2. Copy one of the templates above
3. Replace placeholder values (YOUR_TOKEN, user:pass, etc.)
4. Restart Cursor or reload the project
5. Verify: agent should mention available tools when you ask about them

## Security Notes

- **Never commit API tokens** to git
- Use environment variables: `"env": { "KEY": "$ENV_VAR_NAME" }`
- Add `.cursor/mcp.json` to `.gitignore` if it contains secrets
- Or use a `.cursor/mcp.json.example` with placeholders and `.gitignore` the real one
