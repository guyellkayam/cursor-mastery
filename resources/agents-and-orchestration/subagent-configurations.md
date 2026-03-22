# Subagent Configurations

> **TL;DR**: How to use up to 8 parallel background agents in Cursor — git worktrees, specialized roles, and orchestration patterns.

## What Are Subagents?

Subagents are independent AI agents that run in parallel, each with:
- **Isolated context window** — no cross-contamination between agents
- **Isolated codebase** — each uses a git worktree (separate working copy)
- **Specialized role** — one task per agent
- **Background execution** — doesn't block your main conversation

**Limit**: Up to 8 parallel background agents.

## Built-in Subagents

| Subagent | Purpose | Context |
|----------|---------|---------|
| **Explore** | Searches and analyzes codebase | Read-only access |
| **Bash** | Executes shell commands | Terminal access |
| **Browser** | Manages browser interactions | Web access |

## Custom Subagent Definitions

Create custom subagents in `.cursor/agents/` (project) or `~/.cursor/agents/` (global):

### Security Auditor
```markdown
---
name: security-auditor
description: Reviews code for security vulnerabilities, OWASP Top 10, and dependency issues
model: fast
readonly: true
background: true
---

# Security Auditor

You are a security-focused code reviewer. For every file or change you review:

1. Check for OWASP Top 10 vulnerabilities
2. Verify input validation on all external inputs
3. Check for hardcoded secrets or credentials
4. Review authentication and authorization logic
5. Scan for SQL injection, XSS, CSRF patterns
6. Check dependency versions for known CVEs

Report format:
- CRITICAL: [issue] in [file:line]
- HIGH: [issue] in [file:line]
- MEDIUM: [issue] in [file:line]

Do NOT modify any files — report only.
```

### Test Writer
```markdown
---
name: test-writer
description: Writes comprehensive tests for new features and changes
model: default
readonly: false
background: true
---

# Test Writer

You write tests following existing patterns in the project.

For each feature/change:
1. Find existing test patterns (@codebase test patterns)
2. Write unit tests for core logic
3. Write integration tests for API endpoints
4. Cover edge cases: null, empty, boundary values
5. Run tests to verify they pass

Use the same testing framework and patterns as the existing tests.
Name tests descriptively: test_[what]_[scenario]_[expected]
```

### Documentation Agent
```markdown
---
name: doc-writer
description: Generates and updates documentation based on code changes
model: fast
readonly: false
background: true
---

# Documentation Agent

Update documentation based on recent code changes:

1. Read the git diff to understand what changed
2. Update relevant README sections
3. Update API documentation if endpoints changed
4. Add inline comments only where logic isn't self-evident
5. Update CHANGELOG.md with the change

Follow existing documentation patterns and style.
Do NOT add documentation to files that weren't changed.
```

### DevOps Agent
```markdown
---
name: devops-agent
description: Handles infrastructure, CI/CD, and deployment tasks
model: default
readonly: false
background: true
---

# DevOps Agent

You handle infrastructure and operations tasks:

1. Terraform modules — follow existing module patterns
2. GitHub Actions — use reusable workflows, cache dependencies
3. Kubernetes manifests — follow Helm chart conventions
4. Docker — multi-stage builds, non-root, pinned versions

Always:
- Run terraform fmt, terraform validate after changes
- Use helm lint after chart modifications
- Pin all versions (providers, actions, images)
- Follow GitOps principles (git is source of truth)
```

## Orchestration Patterns

### Pattern 1: Parallel Fan-Out (Research)
```
Main Agent:
"I need to research 3 topics before implementing. Launch subagents:

Background Agent 1: Search for all database migration patterns in this codebase
Background Agent 2: Analyze the current API endpoint structure
Background Agent 3: Review test coverage for the auth module

Report findings. I'll synthesize and create the implementation plan."
```

### Pattern 2: Pipeline (Sequential)
```
Step 1: Test Writer agent → writes failing tests
Step 2: Main agent → implements code to pass tests
Step 3: Doc Writer agent → updates documentation
Step 4: Security Auditor agent → reviews changes
```

### Pattern 3: Hierarchical Team
```
Architect (Main Agent):
  - Designs approach, delegates tasks

Team A (Subagents 1-3):
  - Agent 1: Backend implementation
  - Agent 2: Frontend implementation
  - Agent 3: Test writing

Team B (Subagents 4-5):
  - Agent 4: Documentation
  - Agent 5: Security review

Architect reviews all work, merges, resolves conflicts.
```

### Pattern 4: Git Worktree Isolation
```
Each background agent automatically gets:
- Its own git worktree (isolated copy of the repo)
- Its own branch
- No conflicts with main agent or other subagents

When subagent completes:
- Changes are on its branch
- Main agent can review and merge
- Worktree is cleaned up automatically
```

---

## Configuration

### Project-Level Agents
```
.cursor/agents/
  security-auditor.md
  test-writer.md
  doc-writer.md
```

### Global Agents (Available in all projects)
```
~/.cursor/agents/
  devops-agent.md
  code-reviewer.md
```

## Token Strategy

- Each subagent has its **own context window** — total capacity = 8x single agent
- Use `model: fast` for simple tasks (cheaper, faster)
- Use `readonly: true` for analysis agents (prevents accidental edits)
- Subagent context is **isolated** — it doesn't add to your main agent's token count
- Launch subagents for research → results come back as a summary (compressed)
