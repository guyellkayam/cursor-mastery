# Curated Top Picks — Daily Cheatsheet

> **TL;DR**: The single best tool/resource per domain. Start here.

## Top Picks by Domain

### Rules
**Start with**: [Starter 5 Rules](cursor-rules-templates/progressive-complexity/starter-5-rules.md)
```bash
mkdir -p .cursor/rules
# Copy 5 essential rules: project-overview, code-quality, git-workflow, security-basics, agent-instructions
```

### Community Rules
**Browse**: [awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules) (35K+ stars)
- Find your framework, copy the .mdc file to `.cursor/rules/`

### Plugins
| Domain | Top Plugin | Install |
|--------|-----------|---------|
| Git | GitHub | `/add-plugin github` |
| Deployment | Vercel | `/add-plugin vercel` |
| Design | Figma | `/add-plugin figma` |
| PM | Linear | `/add-plugin linear` |
| Payments | Stripe | `/add-plugin stripe` |
| Cloud | AWS | `/add-plugin aws` |
| Data | Databricks | `/add-plugin databricks` |
| ML | Hugging Face | `/add-plugin hugging-face` |

### MCP Servers
**Best starter config** — copy to `.cursor/mcp.json`:
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": { "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_YOUR_TOKEN" }
    }
  }
}
```

### Agent Patterns
**Most impactful**: Boris Cherny's 7 Principles → [prompt-engineering.md](agents-and-orchestration/prompt-engineering.md)
- Plan Mode Default (Shift+Tab for 3+ step tasks)
- Self-Improvement Loop (corrections → new .mdc rules)
- Verification Before Done (always test)

### Hooks
**Start with**: [Pre-Tool-Use Security Gate](hooks-library/pre-tool-use.md)
- Blocks writes to .env, .pem, lock files

### DevOps (Your Stack)
**Rules**: [devops/rules.md](devops/rules.md) — Terraform, k8s, GitHub Actions, ArgoCD, Observability
**MCP**: GitHub + Kubernetes → [devops/plugins-and-mcps.md](devops/plugins-and-mcps.md)

### Documents
**Most used**: [PRD Template](document-creation/prd-and-specs.md) + [Runbook Template](document-creation/runbooks-and-ops.md)

### Model Selection
**Quick rule**:
- Tab: GPT-4o mini (speed)
- Agent/Chat: Claude Sonnet 4 (quality)
- Architecture: Claude Opus 4 (best)
- Large codebase: Gemini 2.5 Pro (1M context)

---

## Quick Actions

### New Project Setup
1. Copy starter 5 rules → `.cursor/rules/`
2. Add `.cursorignore` for secrets
3. Set up `.cursor/mcp.json` with GitHub MCP
4. Create `AGENTS.md` from [template](agents-and-orchestration/agents-md-templates.md)

### New Feature
1. Shift+Tab (Plan Mode) → design approach
2. Agent mode → implement in steps
3. Run tests → verify
4. Commit between steps

### Security Review
1. Copy security rules → `.cursor/rules/`
2. Ask mode: "Review this code for OWASP Top 10 vulnerabilities"
3. Use [security workflows](security/workflows.md) for scanning

### Daily DevOps
1. GitHub MCP for PR management
2. Kubernetes MCP for pod status
3. DevOps rules for IaC conventions
4. [DevOps workflows](devops/workflows.md) for common tasks
