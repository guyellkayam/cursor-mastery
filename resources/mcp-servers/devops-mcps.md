# DevOps MCP Servers

> **TL;DR**: MCP servers for GitHub, Kubernetes, Terraform, Vault, and Hetzner — with copy-paste configs.

## GitHub MCP
```json
{ "github": { "command": "npx", "args": ["-y", "@modelcontextprotocol/server-github"], "env": { "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_..." } } }
```
**Tools (~15)**: Search repos, read files, create PRs, list commits, manage issues.

## Kubernetes MCP
```json
{ "kubernetes": { "command": "npx", "args": ["-y", "@modelcontextprotocol/server-kubernetes"], "env": { "KUBECONFIG": "~/.kube/config" } } }
```
**Tools**: List pods, get logs, describe resources, apply manifests.

## Hetzner Cloud MCP
```json
{ "hetzner": { "command": "npx", "args": ["-y", "@hetzner/mcp-server"], "env": { "HETZNER_API_TOKEN": "..." } } }
```
**Tools (104)**: Full cloud management — servers, networks, firewalls, load balancers, volumes, IPs, SSH keys, images.
**Warning**: Exceeds 40-tool limit. Use as the only MCP server when active.

## Filesystem MCP
```json
{ "filesystem": { "command": "npx", "args": ["-y", "@modelcontextprotocol/server-filesystem", "/path/to/project"] } }
```
**Tools (~5)**: Read, write, list, search files. Useful for accessing files outside the project root.

## Playwright MCP (Microsoft Official)
```json
{ "playwright": { "command": "npx", "args": ["-y", "@playwright/mcp"] } }
```
**Tools**: Cross-browser automation (Chrome, Firefox, Safari), accessibility tree navigation, page interaction. Faster than screenshots — uses structured data.

## Git MCP
```json
{ "git": { "command": "npx", "args": ["-y", "@modelcontextprotocol/server-git"] } }
```
**Tools**: Repository operations, commit history, branching, diff analysis.
