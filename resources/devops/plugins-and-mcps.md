# DevOps Plugins & MCP Servers

> **TL;DR**: Cursor marketplace plugins and MCP servers for DevOps — GitHub, Kubernetes, Hetzner, AWS, Terraform integration.

## Marketplace Plugins

### GitHub Plugin
**Install**: `/add-plugin github`

- View PRs, issues, and discussions from Cursor
- Create PRs directly from agent
- Review code changes
- Link commits to issues

### AWS Plugin
**Install**: `/add-plugin aws`

- Interact with AWS services from Cursor
- View and manage resources
- Generate IaC from existing resources
- Check costs and billing

### Hetzner Cloud MCP
104 tools across 13 resource domains — servers, networks, firewalls, load balancers.

```json
{
  "mcpServers": {
    "hetzner": {
      "command": "npx",
      "args": ["-y", "@hetzner/mcp-server"],
      "env": {
        "HETZNER_API_TOKEN": "your-token"
      }
    }
  }
}
```

**Warning**: 104 tools exceeds the 40-tool limit. Use with caution or alongside few other MCP servers.

---

## MCP Servers for DevOps

### GitHub MCP
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_..."
      }
    }
  }
}
```

**Tools**: Search repos, read files, create issues, create PRs, list commits.

### Kubernetes MCP
```json
{
  "mcpServers": {
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

**Tools**: List pods, get logs, describe resources, apply manifests.

### Filesystem MCP (for IaC files)
```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/terraform"],
      "env": {}
    }
  }
}
```

---

## DevOps MCP Configuration Example

A balanced DevOps setup (under 40-tool limit):

```json
// .cursor/mcp.json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": {
        "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_..."
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

## "Use this when..."

| Scenario | Plugin/MCP |
|----------|-----------|
| Managing PRs and issues | GitHub plugin or MCP |
| Checking pod status, reading logs | Kubernetes MCP |
| Managing cloud servers | Hetzner MCP or AWS plugin |
| IaC file management | Filesystem MCP |

## Token Strategy

- GitHub MCP is low-tool-count (~15 tools) — efficient for daily DevOps use
- Hetzner MCP has 104 tools — use alone or disable other MCPs when using it
- Use project-level `.cursor/mcp.json` so DevOps MCPs only load in infra projects
- Kubernetes MCP: avoid listing all resources — be specific (namespace, label selector)
