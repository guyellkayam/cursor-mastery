# Cursor Rules Templates

> **TL;DR**: Ready-to-drop `.cursor/rules/` starter kits organized by project type, role, and complexity level. Copy the rules you need into your project.

## How Cursor Rules Work

Cursor uses `.cursor/rules/*.mdc` files to give persistent instructions to the AI agent. Each rule is a Markdown file with YAML frontmatter.

### MDC File Format
```markdown
---
description: Short description of when this rule applies
globs: ["*.ts", "*.tsx"]    # Optional: auto-attach to matching files
alwaysApply: false           # Optional: apply to every request
---

# Rule Title

Your instructions here in Markdown format.
```

### Rule Types
| Type | How it Activates | Best For |
|------|-----------------|----------|
| **Always Apply** | `alwaysApply: true` | Project-wide conventions (naming, commits, testing) |
| **Auto Attached** | `globs: ["*.py"]` | File-type specific rules (Python style, Terraform conventions) |
| **Agent Requested** | Has `description` only | Agent reads description and decides when to use it |
| **Manual** | User invokes explicitly | Rarely-used or context-specific rules |

### Rule Directories
| Location | Scope |
|----------|-------|
| `.cursor/rules/` | Project-level (committed to git, shared with team) |
| `~/.cursor/rules/` | User-level (global, personal preferences) |

## Template Categories

### By Project Type
Ready-to-use rule sets for common project archetypes:

| Template | Files | Description |
|----------|-------|-------------|
| [Microservice](by-project-type/microservice.md) | Container + API + test rules | Docker, REST/gRPC, health checks, observability |
| [Terraform Module](by-project-type/terraform-module.md) | IaC rules | State management, remote backend, security scanning |
| [Python Data Pipeline](by-project-type/python-data-pipeline.md) | ETL rules | Schema validation, idempotency, error handling |
| [Next.js App](by-project-type/nextjs-app.md) | Frontend rules | App Router, RSC, Tailwind, TypeScript strict |
| [Cursor Project Bootstrap](by-project-type/cursor-project-bootstrap.md) | Meta-rules | Rules for setting up rules in a new project |

### By Role
Rules tuned to specific engineering roles:

| Template | Focus |
|----------|-------|
| [DevOps Engineer](by-role/devops-engineer.md) | GitOps, immutable infra, observability, IaC patterns |
| [Security Auditor](by-role/security-auditor.md) | Threat modeling, compliance, OWASP, scanning rules |
| [Data Analyst](by-role/data-analyst.md) | Reproducibility, notebook hygiene, data validation |
| [Financial Analyst](by-role/financial-analyst.md) | Model accuracy, data sources, audit trails |

### By Complexity (Progressive)
Start minimal, add as needed:

| Level | Rules | Description |
|-------|-------|-------------|
| [Starter (5 rules)](progressive-complexity/starter-5-rules.md) | 5 essential rules | Works for any project — start here |
| [Intermediate (15 rules)](progressive-complexity/intermediate-15-rules.md) | 15 rules | Covers most scenarios for mature projects |
| [Expert (30 rules)](progressive-complexity/expert-30-rules.md) | 30 rules | Full Boris Cherny + domain-specific extensions |

## Quick Start

```bash
# 1. Create rules directory
mkdir -p .cursor/rules

# 2. Copy starter rules (pick one approach):

# Option A: Start minimal (recommended)
# Copy the 5 starter rules from progressive-complexity/starter-5-rules.md

# Option B: By project type
# Copy rules from by-project-type/microservice.md

# Option C: By role
# Copy rules from by-role/devops-engineer.md
```

## Best Practices

1. **Start with 5 rules** — Only add rules when the agent makes repeated mistakes
2. **Keep rules under 500 lines** — Split large rules into smaller focused files
3. **Use globs for scoping** — `globs: ["*.tf"]` keeps Terraform rules out of Python files (saves tokens)
4. **Version control rules** — Commit `.cursor/rules/` to git for team alignment
5. **Kebab-case filenames** — Use `import-ordering.mdc`, not `ImportOrdering.mdc`
6. **One concern per rule** — Don't mix testing rules with style rules

## Token Strategy

- Rules with `globs` only load when matching files are open — massive token savings
- `alwaysApply: true` loads on every request — use sparingly (max 3-5 always-apply rules)
- The agent reads rule `description` fields to decide relevance — write clear descriptions
- More rules ≠ better — each rule consumes context. Start with 5, add as needed.
