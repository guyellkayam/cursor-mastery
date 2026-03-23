# MCP Servers for Cursor

> **TL;DR**: Model Context Protocol servers give Cursor agent access to external tools — databases, APIs, cloud services. Configure in `.cursor/mcp.json`.

## Ecosystem (March 2026)

- **10,000+** public MCP servers across the ecosystem
- **MCP TypeScript SDK v1.27** — 34,700+ dependent npm projects
- **Governance**: MCP donated to Agentic AI Foundation (Linux Foundation) — co-founded by Anthropic, Block, OpenAI
- **Google**: Official managed MCP servers for all Google/Google Cloud services
- **FastMCP**: Python framework for building MCP servers (featured on Thoughtworks Technology Radar)
- **MCP Apps**: Cursor 2.6 renders interactive UIs from MCP servers directly in chat

## How MCP Works in Cursor

1. MCP servers expose **tools** that the agent can call
2. Cursor sends only the **first 40 tools** to the agent (limit!)
3. Configure in `.cursor/mcp.json` (project) or `~/.cursor/mcp.json` (global)
4. Transport types: `stdio` (local), `sse`/`streamable-http` (remote)

## Files

| File | What's Inside |
|------|--------------|
| [DevOps MCPs](devops-mcps.md) | GitHub, Kubernetes, Terraform, Vault, Hetzner |
| [Productivity MCPs](productivity-mcps.md) | Atlassian, Google Calendar, Gmail, Linear |
| [Data MCPs](data-mcps.md) | PostgreSQL, Redis, AlphaVantage, Hasura |
| [AI MCPs](ai-mcps.md) | Hugging Face, Ollama, Anthropic API |
| [Config Examples](config-examples/mcp-json-templates.md) | Copy-paste mcp.json per stack |

## Quick Start

```json
// .cursor/mcp.json
{
  "mcpServers": {
    "server-name": {
      "command": "npx",
      "args": ["-y", "@package/name"],
      "env": {
        "API_KEY": "your-key"
      }
    }
  }
}
```

## 40-Tool Limit Strategy

Cursor only sends the first 40 tools. Manage this:
- **Project-level**: Only enable MCPs relevant to current project
- **Global-level**: Minimal set (GitHub + one other)
- **High tool count servers**: Hetzner (104), Atlassian (50+) — use alone
- **Low tool count**: GitHub (~15), Filesystem (~5) — combine freely
- **MCP Hub Wrapper**: Use `mcp-hub-mcp` to bypass the limit entirely (see below)

### MCP Hub — Bypass the 40-Tool Limit

`mcp-hub-mcp` proxies ALL configured servers through just 2 tools:
- `list-all-tools` — discover available tools
- `call-tool` — invoke any tool by name

```json
{
  "mcpServers": {
    "mcp-hub": {
      "command": "npx",
      "args": ["-y", "mcp-hub-mcp"]
    }
  }
}
```
**Trade-off**: Slightly slower invocation, but unlimited tools.

## Top MCP Servers by Usage (FastMCP Analytics, March 2026)

| Rank | Server | Views | Category |
|------|--------|-------|----------|
| 1 | **Context7** | 11,000+ | Memory management |
| 2 | **Playwright** | ~6,000 | Browser automation |
| 3 | **Desktop Commander** | — | Terminal & file ops |

## Registries & Discovery

| Registry | URL | Servers |
|----------|-----|---------|
| Official MCP Registry | [registry.modelcontextprotocol.io](https://registry.modelcontextprotocol.io/) | Official + verified |
| MCP.so | [mcp.so](https://mcp.so/) | Community marketplace (10K+) |
| MCP Servers Directory | [mcpservers.org](https://mcpservers.org/) | 5,000+ community |
| FastMCP Directory | [fastmcp.me](https://fastmcp.me/) | 1,864+ servers with analytics |
| PulseMCP | [pulsemcp.com](https://www.pulsemcp.com/) | Curated collection |
| Awesome MCP Servers | [github.com/wong2/awesome-mcp-servers](https://github.com/wong2/awesome-mcp-servers) | GitHub awesome list |
| cursor.directory | [cursor.directory](https://cursor.directory) | Cursor-specific |

## Token Strategy

- Each MCP tool definition costs **300-600 tokens** in the system prompt
- 10 servers × 15 tools × 500 tokens = **75,000 tokens** (1/3 of context window!)
- Fewer MCP servers = dramatically lower baseline token cost
- Use project-level config so DevOps MCPs don't load in frontend projects
- Use `mcp-hub-mcp` wrapper to reduce to just 2 tool definitions regardless of server count
