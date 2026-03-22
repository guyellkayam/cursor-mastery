# Cursor Project Bootstrap Rules

> **TL;DR**: Meta-rules for setting up a new project with Cursor AI. Use this when starting a fresh project to get the right rules, skills, and agent config in place.

## Rules Set (Copy to `.cursor/rules/`)

### bootstrap.mdc
```markdown
---
description: Bootstrap a new project with Cursor AI conventions
alwaysApply: true
---

# Cursor Project Bootstrap

## When setting up a new project:

### 1. Create Rule Directory
```bash
mkdir -p .cursor/rules
```

### 2. Start with 5 Essential Rules
Copy from cursor-mastery/resources/cursor-rules-templates/progressive-complexity/starter-5-rules.md:
- project-overview.mdc
- code-quality.mdc
- git-workflow.mdc
- security-basics.mdc
- agent-instructions.mdc

### 3. Add .cursorignore
```
.env
.env.*
*.pem
*.key
*.cert
node_modules/
__pycache__/
.terraform/
terraform.tfstate*
*.tfvars
.git/
dist/
build/
```

### 4. Configure MCP Servers
Create .cursor/mcp.json with project-relevant servers:
```json
{
  "mcpServers": {}
}
```

### 5. Create AGENTS.md (if using subagents)
```markdown
# Project Agent Documentation

## Architecture
[Brief architecture description]

## Code Standards
[Key coding standards]

## Testing
[Testing requirements and patterns]
```

### 6. Add Project-Specific Rules
Only when the agent makes repeated mistakes:
- Framework rules (Next.js, FastAPI, etc.)
- Domain rules (DevOps, finance, etc.)
- Team conventions (PR format, review process)

## Rule Addition Criteria
Add a new rule ONLY when:
1. The agent makes the same mistake 3+ times
2. A team convention can't be inferred from code
3. A security pattern must be enforced

Do NOT add rules for:
- Things the agent already does well
- Patterns obvious from the codebase
- One-time instructions (use the chat instead)
```

### gitignore-cursor.mdc
```markdown
---
description: Cursor-specific files to include/exclude from git
alwaysApply: true
---

# Cursor Git Configuration

## Commit to git (share with team):
- .cursor/rules/*.mdc — Project rules
- .cursor/mcp.json — MCP server config (without secrets)
- .cursor/skills/ — Project skills
- .cursorignore — Context exclusions
- AGENTS.md — Agent documentation

## Do NOT commit:
- .cursor/plans/ — Session-specific plans
- .cursor/change-log.txt — Local change tracking
- .cursor/progress.md — Session progress
- .cursor/logs/ — Cost tracking logs
- .cursor/subagent-tracker.json — Subagent state

## .gitignore additions:
```
.cursor/plans/
.cursor/change-log.txt
.cursor/progress.md
.cursor/logs/
.cursor/subagent-tracker.json
```
```
