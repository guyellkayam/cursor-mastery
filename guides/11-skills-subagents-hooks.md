# Guide 11: Skills, Subagents & Hooks

> **Level**: 7-9
> **Prerequisites**: Guides 01-06
> **Official Docs**: https://cursor.com/docs/customizing/skills, /subagents, /hooks

## Skills

Skills are portable, version-controlled packages that teach agents how to perform domain-specific tasks. They're like plugins for the AI — giving it specialized knowledge and workflows.

### How Skills Work
- Auto-load from designated directories
- Load resources on demand (efficient context usage)
- Work across any agent supporting the Agent Skills standard

### Skill Directories
```
.cursor/skills/       # Project-level (committed to repo)
.agents/skills/       # Project-level (alternative)
~/.cursor/skills/     # User-level (global, all projects)
```

Legacy compatibility: `.claude/skills/`, `.codex/skills/` also work.

### Creating a Skill

Each skill is a folder with a `SKILL.md` file:

```
.cursor/skills/
  my-skill/
    SKILL.md          # Required: frontmatter + instructions
    scripts/           # Optional: executable code
    references/        # Optional: documentation
    assets/            # Optional: templates, static files
```

**SKILL.md format:**
```markdown
---
name: react-components
description: Generates React components following our design system patterns with TypeScript, Tailwind, and Storybook stories.
---

# React Component Skill

When creating components:
1. Use functional components with TypeScript
2. Apply Tailwind classes from our design tokens
3. Generate Storybook stories alongside
4. Include unit tests with Testing Library

## Template
@file:assets/component-template.tsx
```

### Activation Modes

| Mode | How | Use Case |
|------|-----|----------|
| **Automatic** | Agent decides based on description | Default — smart matching |
| **Manual** | Type `/skill-name` in chat | Explicit control |
| **Slash-only** | `disable-model-invocation: true` | Prevent auto-activation |

### Managing Skills
- **Settings > Rules > Agent Decides**: See all discovered skills
- **Install remote skills**: Via GitHub URLs in settings
- Type `/` in Agent to see available skills and select manually

### Best Practices
- Write clear, specific descriptions (the AI matches tasks to skills by description)
- Keep SKILL.md focused — use `references/` for detailed docs
- Include concrete examples and templates in `assets/`
- Version control skills with your project

---

## Subagents

Subagents are specialized AI assistants that handle delegated tasks in isolated context windows. They're like spawning worker threads for the AI — each with its own context, focus, and capabilities.

### Why Subagents?
- **Context Isolation**: Long research tasks don't consume your main conversation
- **Parallel Execution**: Multiple subagents run simultaneously
- **Specialized Focus**: Custom prompts, tools, and models per task
- **Reusability**: Define once, use across projects

### Built-in Subagents

Cursor provides three automatic subagents:

| Subagent | What It Does | Model |
|----------|-------------|-------|
| **Explore** | Searches and analyzes codebase | Fast model (many parallel searches) |
| **Bash** | Executes shell commands | Isolates verbose terminal output |
| **Browser** | Manages browser interactions | Filters noisy DOM data |

These activate automatically — no configuration needed.

### Custom Subagent Creation

**File Locations:**
```
.cursor/agents/       # Project-level
~/.cursor/agents/     # User-level (global)
```

Also supports: `.claude/agents/`, `.codex/agents/`

**Subagent file format (Markdown + YAML frontmatter):**

```markdown
---
name: security-auditor
description: Reviews code for security vulnerabilities including injection, XSS, auth bypass. Verifies secrets aren't hardcoded.
model: fast
readonly: true
background: false
---

# Security Auditor

You are a security-focused code reviewer. When invoked:

1. Scan all changed files for OWASP Top 10 vulnerabilities
2. Check for hardcoded secrets, API keys, tokens
3. Verify input validation on all user-facing endpoints
4. Check authentication and authorization logic
5. Report findings with severity levels (Critical/High/Medium/Low)

Never suggest changes — only report findings.
```

### Configuration Fields

| Field | Type | Description |
|-------|------|-------------|
| `name` | string | Unique identifier (lowercase, hyphens) |
| `description` | string | When to use this subagent |
| `model` | string | `"fast"`, `"inherit"`, or specific model ID |
| `readonly` | boolean | Restrict write permissions |
| `background` | boolean | Run without blocking |

### Execution Modes

| Mode | Behavior | Use For |
|------|----------|---------|
| **Foreground** | Blocks until complete | Sequential tasks needing results |
| **Background** | Returns immediately | Long-running or parallel work |

### Invoking Subagents

**Automatic**: Agent delegates based on task complexity and descriptions.

**Explicit**:
```
/security-auditor review the auth module
Have the debugger investigate this error
```

**Parallel**: Multiple subagents launch simultaneously.

### Common Subagent Patterns

**Verification Agent:**
```markdown
---
name: verifier
description: Independently validates whether claimed work was actually completed. Tests implementations and identifies gaps.
model: fast
readonly: true
---
# Verify all changes. Run tests. Check edge cases.
# Report: PASS/FAIL with specific evidence.
```

**Test Runner:**
```markdown
---
name: test-runner
description: Runs tests and fixes failures. Analyzes output, identifies root causes, fixes while preserving test intent.
model: inherit
---
# Run the test suite. For failures:
# 1. Analyze the failure output
# 2. Identify root cause
# 3. Fix the implementation (not the test)
# 4. Re-run to confirm
```

**Orchestrator Pattern:**
```
Planner (plan) → Implementer (build) → Verifier (validate)
```

### Best Practices
- Single responsibility per subagent
- Write detailed descriptions (drives auto-delegation)
- Concise, specific prompts
- Commit to version control for team use
- Start with auto-generated drafts, then customize

### Limitations
- No nested subagents (a subagent can't spawn subagents)
- Each subagent consumes its own token budget
- 5 parallel subagents = ~5x token usage
- Simple tasks may be slower with subagent overhead

---

## Hooks

Hooks are not fully documented in public Cursor docs yet. Based on the Skills docs and community patterns, hooks in Cursor work similarly to pre/post scripts:

### How Hooks Work (Based on Skills Integration)
Skills can include hooks — scripts that run before or after agent actions:

```
.cursor/skills/
  my-skill/
    SKILL.md
    scripts/
      pre-commit.sh      # Runs before git commit
      post-edit.sh        # Runs after file edits
      lint-check.sh       # Custom validation
```

### Hook Configuration in Skills
In SKILL.md, reference hook scripts:
```markdown
---
name: quality-gate
description: Enforces code quality with automatic linting and testing after edits.
---

## Hooks
- After editing TypeScript files, run: `scripts/lint-check.sh`
- Before commit, run: `scripts/pre-commit.sh`
```

### Community Patterns
From the Cursor Blog's best practices, hooks enable:
- **Long-running loops**: Auto-continue agent work until tests pass
- **Post-edit validation**: Lint/format after file changes
- **Pre-commit checks**: Security scans before committing

## Sources
- [Cursor Docs: Skills](https://cursor.com/docs/customizing/skills)
- [Cursor Docs: Subagents](https://cursor.com/docs/customizing/subagents)
- [Cursor Blog: Agent Best Practices](https://cursor.com/blog/agent-best-practices)
