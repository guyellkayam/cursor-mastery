# Ecosystem Update — 2026-03-23

## Cursor Editor Updates

### Cursor 2.6 (March 3, 2026)
- **MCP Apps**: Interactive UIs rendered directly in agent chats — charts from Amplitude, diagrams from Figma, whiteboards from tldraw
- **Team Marketplaces**: Admins on Teams/Enterprise can create private plugin marketplaces with central governance

### Automations (March 5, 2026)
- Always-on agents that run based on triggers (Slack, Linear, GitHub, PagerDuty, webhooks)
- Agent spins up cloud sandbox with configured MCPs and models
- Memory tool lets agents learn from past runs
- Hundreds of automations run per hour at Cursor

### 30+ New Plugins (March 11, 2026)
- Partners: Atlassian, Datadog, GitLab, Glean, Hugging Face, monday.com, PlanetScale
- Plugins contain MCPs usable by cloud agents and automations

### Composer 2 (March 19, 2026)
- Cursor's own frontier-level coding model
- Pricing: Standard ($0.50/M input, $2.50/M output), Fast ($1.50/M input, $7.50/M output)
- Reports strong results on challenging coding tasks

### Models Supported (March 2026)
- **Anthropic**: Claude Sonnet 4.5, Opus 4.6
- **OpenAI**: GPT-5.4, GPT-5.3, GPT-5.2, GPT-4.1 (1M context)
- **Google**: Gemini 3 Pro, Gemini 3 Flash
- **Cursor**: Composer 2 (claimed 4x faster than competitors)

### Pricing (Current)
| Plan | Price | Notes |
|------|-------|-------|
| Hobby | Free | Limited agent requests + tab completions |
| Pro | $20/mo | $20 credit pool (usage-based since June 2025) |
| Pro+ | $60/mo | Higher credit pool |
| Ultra | $200/mo | Maximum credits |
| Teams | $40/user/mo | Team features + admin controls |

## BugBot Updates
- **GA release**: February 25, 2026
- Fully agentic architecture (fall 2025 migration from pipeline)
- 76% resolution rate (up from 52%)
- 2M+ PRs/month reviewed
- 35%+ of Autofix changes merged
- Customers: Rippling, Discord, Samsara, Airtable, Sierra AI
- Custom review rules and specialized automations (e.g., agentic security review → Slack)

## MCP Ecosystem

### Governance
- MCP donated to Agentic AI Foundation (AAIF, Linux Foundation) — Dec 2025
- Co-founded by Anthropic, Block, OpenAI
- Formal governance: Working Groups, Spec Enhancement Proposals (SEPs)

### SDK & Growth
- TypeScript SDK v1.27 (Feb 2026) — 34,700+ dependent npm projects
- 10,000+ public MCP servers
- FastMCP Python framework on Thoughtworks Technology Radar

### 2026 Roadmap Priorities
1. Transport & server discovery (`.well-known` metadata)
2. Agentic lifecycle (Tasks primitive, retry semantics, expiry policies)
3. Governance maturation (contributor ladder, Working Group delegation)
4. Enterprise readiness (audit trails, SSO, gateways)

### Google MCP
- Official managed MCP servers for all Google/Google Cloud services
- Colab MCP Server — open source, connects any AI agent to Google Colab

### Top MCP Servers (FastMCP analytics)
1. **Context7** — 11K+ views, memory management
2. **Playwright** — ~6K views, browser automation
3. **Desktop Commander** — terminal & file operations

## Hooks System (Verified)
- Config file: `.cursor/hooks.json` (project) or `~/.cursor/hooks.json` (global)
- Format: `{ "version": 1, "hooks": { ... } }`
- 6 lifecycle events: `beforeSubmitPrompt`, `beforeShellExecution`, `beforeMCPExecution`, `beforeReadFile`, `afterFileEdit`, `stop`
- Scripts receive JSON on stdin, return JSON on stdout
- All hooks from all config locations are merged and executed

## Environment.json (Verified)
- Location: `.cursor/environment.json`
- Fields: `baseImage`, `install`, `start`, `terminals` (name/command/ports), `env`, `persistedDirectories`

## Community
- cursor.directory: 75,100+ developers
- awesome-cursorrules: 31.3K+ stars, 163+ rules files
- Cursor marketplace: 130+ plugins (launched Feb 17, 2026)

## Gaps Resolved
- Marketplace plugin details (Render, Clerk, JFrog, Miro, Meta Reality Labs, Plain, Circle, Phantom)
- cursor.directory live data
- Hooks configuration format (.cursor/hooks.json)
- MCP Hub wrapper (mcp-hub-mcp confirmed real)
- Cloud agent environment.json fields

## New Gaps Identified
- Automations deep dive needed
- Composer 2 benchmarks/pricing details
- MCP Apps documentation
- Team marketplace setup guide
- Pricing guide with per-model token costs

## Sources
- [Cursor Changelog](https://cursor.com/changelog)
- [Cursor Blog: Bugbot Autofix](https://cursor.com/blog/bugbot-autofix)
- [Cursor Docs: Hooks](https://cursor.com/docs/hooks)
- [Cursor Docs: Cloud Agent Setup](https://cursor.com/docs/cloud-agent/setup)
- [MCP 2026 Roadmap](https://modelcontextprotocol.io/development/roadmap)
- [FastMCP: Top MCP Servers](https://fastmcp.me/blog/top-10-most-popular-mcp-servers)
- [Google Cloud: MCP Support](https://cloud.google.com/blog/products/ai-machine-learning/announcing-official-mcp-support-for-google-services)
- [CNBC: Cursor Agent Update](https://www.cnbc.com/2026/02/24/cursor-announces-major-update-as-ai-coding-agent-battle-heats-up.html)
- [TechCrunch: Cursor Automations](https://techcrunch.com/2026/03/05/cursor-is-rolling-out-a-new-system-for-agentic-coding/)
