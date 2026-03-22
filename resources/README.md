# Cursor AI Resources Knowledge Base

> **TL;DR**: Curated catalog of rules, plugins, MCP servers, agent patterns, and workflows for Cursor AI — organized by domain for daily use.

## Quick Navigation

| Domain | What's Inside | Start Here |
|--------|--------------|------------|
| [Rules Library](rules-library/) | .cursorrules & .mdc collections by language, framework, domain | [by-language.md](rules-library/by-language.md) |
| [Marketplace Plugins](marketplace-plugins/) | 100+ plugins: Figma, AWS, Linear, Stripe, Vercel... | [development.md](marketplace-plugins/development.md) |
| [MCP Servers](mcp-servers/) | 75+ servers with copy-paste .cursor/mcp.json configs | [devops-mcps.md](mcp-servers/devops-mcps.md) |
| [DevOps](devops/) | Tailored to: k3s, ArgoCD, Vault, Terraform, GitHub Actions | [workflows.md](devops/workflows.md) |
| [Finance](finance/) | Equity research, FinOps, AlphaVantage, Stripe, budgets | [workflows.md](finance/workflows.md) |
| [Security](security/) | OWASP rules, Kyverno, Trivy, Falco, shift-left patterns | [rules.md](security/rules.md) |
| [Python](python/) | FastAPI, Django, data science, pytest, type hints | [rules.md](python/rules.md) |
| [Project Management](project-management/) | Linear, Atlassian, monday.com, sprint planning | [plugins.md](project-management/plugins.md) |
| [Document Creation](document-creation/) | PRD, runbooks, API docs, release notes, compliance | [prd-and-specs.md](document-creation/prd-and-specs.md) |
| [Agents & Orchestration](agents-and-orchestration/) | Agent modes, subagents, cloud agents, AGENTS.md templates | [agent-mode-patterns.md](agents-and-orchestration/agent-mode-patterns.md) |
| [Hooks Library](hooks-library/) | Pre/post tool use, session hooks, notifications | [pre-tool-use.md](hooks-library/pre-tool-use.md) |
| [Cursor Rules Templates](cursor-rules-templates/) | Ready-to-drop .cursor/rules/ starter kits (by project, role, complexity) | [progressive-complexity/starter-5-rules.md](cursor-rules-templates/progressive-complexity/starter-5-rules.md) |
| [AI & ML](ai-and-ml/) | Model selection, local LLMs, Hugging Face, RAG | [model-selection.md](ai-and-ml/model-selection.md) |
| [Data Analysis](data-analysis/) | Databricks, Snowflake, Hex, CSV/Excel analysis | [tools-and-workflows.md](data-analysis/tools-and-workflows.md) |
| [Cloud & Infra](cloud-and-infra/) | AWS, Azure, Cloudflare, Vercel, Netlify, Hetzner | [plugins-and-rules.md](cloud-and-infra/plugins-and-rules.md) |
| [Git & GitHub](git-and-github/) | GitHub plugin, PR workflows, CODEOWNERS | [workflows.md](git-and-github/workflows.md) |
| [Databases](databases/) | PostgreSQL, Redis, PlanetScale, Hasura MCP servers | [mcps-and-rules.md](databases/mcps-and-rules.md) |
| [Community Index](community-index/) | awesome-cursorrules (35k+), cursor.directory, forum guides | [awesome-cursorrules-digest.md](community-index/awesome-cursorrules-digest.md) |

## How Resources Are Organized

Every resource file follows the same format:

1. **TL;DR** — One sentence: what it is and when to use it
2. **Quick Install** — Copy-paste install command (`/add-plugin`, `.cursor/mcp.json`, or `.cursor/rules/`)
3. **Curated Items** — Table with name, source, use case, install
4. **"Use this when..."** — Scenario-based guidance
5. **Token Strategy** — How to use without blowing context (rule scoping via globs, 40-tool MCP limit)
6. **Workflows** — Copy-paste ready patterns for Cursor agent mode

## Install Methods at a Glance

| Method | When to Use | Example |
|--------|------------|---------|
| `/add-plugin` | Marketplace plugins | `/add-plugin figma` |
| `.cursor/rules/*.mdc` | Project rules | Copy `.mdc` file to `.cursor/rules/` |
| `.cursor/mcp.json` | MCP servers | Add server config to JSON file |
| `SKILL.md` | Custom skills | Create `SKILL.md` in `.cursor/skills/` |
| `AGENTS.md` | Agent documentation | Create at project root or subsystem level |

## Daily Cheatsheet

See [curated-top-picks.md](curated-top-picks.md) for the **top 3-5 tools per domain** with install commands.

## Source Repositories

| Source | Stars | What it provides |
|--------|-------|-----------------|
| [awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules) | 35k+ | Massive rules collection across frameworks |
| [cursor.directory](https://cursor.directory) | 75k+ devs | Plugins, MCP servers, community hub |
| [Cursor Marketplace](https://cursor.com/marketplace) | Official | Figma, AWS, Linear, Stripe, Vercel... |
| [Cursor Docs](https://cursor.com/docs) | Official | Agent, MCP, CLI documentation |
| [Cursor Learn](https://cursor.com/learn) | Official | Agent tutorials and customization |
| [awesome-cursor-rules-mdc](https://github.com/sanjeed5/awesome-cursor-rules-mdc) | Community | MDC format rules + reference guide |
| [cursor-security-rules](https://github.com/matank001/cursor-security-rules) | Community | Security-focused rules |
| [cursor-best-practices](https://github.com/digitalchild/cursor-best-practices) | Community | Best practices collection |
