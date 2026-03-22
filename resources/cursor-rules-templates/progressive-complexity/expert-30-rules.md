# Expert: 30 Rules

> **TL;DR**: Full Boris Cherny orchestration principles + domain-specific extensions. For power users running multi-agent workflows with advanced token optimization.

## Includes All 15 Intermediate Rules

See [starter-5-rules.md](starter-5-rules.md) (rules 1-5) and [intermediate-15-rules.md](intermediate-15-rules.md) (rules 6-15).

---

## Boris Cherny Orchestration Rules (16-22)

### Rule 16: Plan Mode Default

```markdown
---
description: Enforce Plan Mode for complex tasks
alwaysApply: true
---

# Plan Mode Default

Use Plan Mode (Shift+Tab) for ANY task that involves:
- 3+ steps or files
- Architectural decisions
- New feature additions
- Refactoring

Workflow: Research → Plan → Wait for approval → Implement in steps → Verify
```

### Rule 17: Subagent Strategy

```markdown
---
description: Delegate to background agents for parallel work
alwaysApply: true
---

# Subagent Strategy

Use background agents (up to 8 parallel) for:
- Research across codebase
- Parallel test + implementation
- Documentation generation
- Code review

One task per agent. Each agent gets isolated context via git worktrees.
```

### Rule 18: Self-Improvement Loop

```markdown
---
description: Create rules from corrections
alwaysApply: true
---

# Self-Improvement Loop

After ANY correction:
1. Fix the issue
2. Create a new .cursor/rules/*.mdc capturing the pattern
3. Reference the new rule in your response

Same mistake must never happen twice.
```

### Rule 19: Verification Before Done

```markdown
---
description: Never mark complete without verification
alwaysApply: true
---

# Verification Before Done

Before declaring complete: build succeeds, tests pass, lint clean, types check.
If ANY verification fails, fix and re-verify. Never say "done" without proof.
```

### Rule 20: Demand Elegance

```markdown
---
description: Seek simpler solutions before presenting
alwaysApply: true
---

# Demand Elegance

For non-trivial changes: write first solution, PAUSE, ask "is there a simpler way?"
Prefer existing patterns, standard library, composition. Refactor before presenting.
```

### Rule 21: Autonomous Bug Fixing

```markdown
---
description: Fix bugs without hand-holding
alwaysApply: true
---

# Autonomous Bug Fixing

Given a bug: reproduce → locate → understand → fix → verify → prevent (add regression test).
Search autonomously. Only ask if bug report is ambiguous about expected behavior.
```

### Rule 22: Task Management Workflow

```markdown
---
description: Structured task execution workflow
alwaysApply: true
---

# Task Management

1. Plan (research + approach)
2. Verify Plan (edge cases, risks)
3. Track Progress (checkpoints, commits between steps)
4. Explain Changes (what + why)
5. Document Results (update docs if behavior changed)
6. Capture Lessons (new rule if something surprising happened)
```

---

## Advanced Token Optimization Rules (23-25)

### Rule 23: Context Scoping

```markdown
---
description: Minimize token usage through smart context management
alwaysApply: true
---

# Context Scoping

- Let the agent search with @codebase — don't paste large files manually
- Use @file only for specific, relevant files
- Start new conversations per task — don't accumulate stale context
- Use Ask mode for exploration before Agent mode for changes
- Scope rules with globs — only load rules when relevant files are open
```

### Rule 24: Efficient Agent Delegation

```markdown
---
description: Delegate efficiently to minimize total token usage
alwaysApply: true
---

# Efficient Delegation

- Use subagents for research — they have isolated context windows
- Main agent focuses on architecture decisions only
- Don't over-specify context for subagents — let them search
- Close completed conversations — don't keep them open
- Use Plan mode output as the source of truth, not conversation history
```

### Rule 25: MCP Tool Budget

```markdown
---
description: Manage MCP tool count for optimal performance
alwaysApply: true
---

# MCP Tool Budget

Cursor sends only the first 40 MCP tools to the agent. Strategy:
- Only enable MCP servers you actively use
- Prefer servers with fewer, focused tools over servers with many tools
- Use project-level .cursor/mcp.json for project-specific servers
- Use global ~/.cursor/mcp.json for always-needed servers
- Disable unused servers to stay under the 40-tool limit
```

---

## Domain-Specific Rules (26-30)

### Rule 26: Terraform / IaC

```markdown
---
description: Infrastructure as Code conventions
globs: ["*.tf", "*.tfvars", "terraform/**"]
---

# Terraform Conventions

- Use modules for reusable infrastructure
- Remote state with locking (S3 + DynamoDB or Terraform Cloud)
- Variables with descriptions, types, and validation
- Outputs for values consumed by other modules
- Use data sources instead of hardcoded IDs
- terraform fmt before every commit
- terraform validate in CI
- Use tfsec/checkov for security scanning
- Pin provider versions
```

### Rule 27: Kubernetes Manifests

```markdown
---
description: Kubernetes manifest conventions
globs: ["*.yaml", "*.yml", "k8s/**", "manifests/**", "helm/**"]
---

# Kubernetes Conventions

- Use namespaces for isolation
- Set resource requests AND limits for all containers
- Use readiness AND liveness probes
- Use ConfigMaps for config, Secrets for sensitive data
- Use labels consistently: app, version, environment
- Use Helm charts for templating
- Use NetworkPolicies for pod-to-pod security
- Use PodDisruptionBudgets for availability
- Pin image tags — never use :latest in production
```

### Rule 28: GitHub Actions

```markdown
---
description: GitHub Actions workflow conventions
globs: [".github/workflows/**"]
---

# GitHub Actions Rules

- Name workflows descriptively
- Use matrix strategy for multi-version testing
- Cache dependencies (actions/cache)
- Pin action versions to commit SHA or tag
- Use repository secrets — never hardcode
- Fail fast on first error
- Use concurrency groups to prevent duplicate runs
- Separate CI (test) from CD (deploy) workflows
- Use reusable workflows for common patterns
- Add timeout-minutes to prevent hanging jobs
```

### Rule 29: Observability

```markdown
---
description: Monitoring and observability patterns
globs: ["**/monitoring/**", "**/alerts/**", "prometheus/**", "grafana/**"]
---

# Observability Rules

- Use RED method for services (Rate, Errors, Duration)
- Use USE method for resources (Utilization, Saturation, Errors)
- PromQL: rate() for counters, avg_over_time() for gauges
- Alert on symptoms (high error rate), not causes (high CPU)
- Every alert must have a runbook link
- Dashboard: overview → drill-down → details
- Log format: structured JSON with timestamp, level, message, context
- Trace context propagation across service boundaries
```

### Rule 30: Security Hardening

```markdown
---
description: Advanced security rules for production systems
alwaysApply: false
---

# Security Hardening

- Principle of least privilege for all service accounts
- Network policies: deny-all default, allow specific
- Pod Security Standards: restricted profile
- Image scanning in CI (Trivy, Grype)
- Runtime security monitoring (Falco)
- Policy enforcement (Kyverno, OPA/Gatekeeper)
- Secret rotation strategy
- Audit logging for all administrative actions
- mTLS for service-to-service communication
- Regularly update base images and dependencies
```

---

## Installation

```bash
mkdir -p .cursor/rules

# Copy rules 1-15 from starter and intermediate
# Then add rules 16-30 from this file
# Customize globs and content for your project

# Recommended: Start with starter-5, add rules as needed
# This full set is for projects where you've already identified
# the need for comprehensive governance
```

## Token Impact

With 30 rules, token usage is significant. Mitigation:
- Only 5 rules are `alwaysApply: true` (Boris Cherny core)
- 10 rules use `globs` — only load when matching files open
- 15 rules are `Agent Requested` — agent decides when to load based on description
- **Estimated overhead**: ~2,000 tokens for always-apply rules + ~500 per activated glob rule
- **Break-even**: Rules prevent repeat corrections, each of which costs ~1,000+ tokens
