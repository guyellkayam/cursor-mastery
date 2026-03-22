# Starter: 5 Essential Rules

> **TL;DR**: The minimum viable rule set for any project. Copy these 5 rules to `.cursor/rules/` and you're set.

## Rule 1: Project Overview

```markdown
---
description: Project overview and conventions
alwaysApply: true
---

# Project Overview

## About
[PROJECT_NAME] is a [brief description].

## Tech Stack
- Language: [e.g., TypeScript 5, Python 3.12]
- Framework: [e.g., Next.js 15, FastAPI]
- Database: [e.g., PostgreSQL, Redis]
- Infrastructure: [e.g., Docker, Kubernetes, Terraform]

## Key Conventions
- Use [style guide] for code formatting
- Tests go in [test directory path]
- Commits follow Conventional Commits format
- Branch naming: feature/TICKET-description, fix/TICKET-description

## Directory Structure
```
src/           — Application source code
tests/         — Test files (mirror src/ structure)
docs/          — Documentation
.cursor/rules/ — AI agent rules
```
```

---

## Rule 2: Code Quality

```markdown
---
description: Code quality and style conventions
alwaysApply: true
---

# Code Quality Standards

## General
- Write simple, readable code — prefer clarity over cleverness
- Functions should do one thing well
- Use descriptive names (variables, functions, files)
- Keep files under 300 lines — split if larger
- No commented-out code — delete it (git has history)

## Error Handling
- Handle errors explicitly — no silent failures
- Use specific error types, not generic catches
- Log errors with context (what happened, what was expected)

## Testing
- Write tests for new features and bug fixes
- Test file mirrors source file path (src/auth.ts → tests/auth.test.ts)
- Test behavior, not implementation details
- Run existing tests before marking work complete
```

---

## Rule 3: Git Workflow

```markdown
---
description: Git and commit conventions
alwaysApply: true
---

# Git Workflow

## Commit Messages
Follow Conventional Commits:
- feat(scope): add new feature
- fix(scope): fix a bug
- docs(scope): documentation changes
- refactor(scope): code refactoring
- test(scope): add or update tests
- chore(scope): maintenance tasks

## Branch Strategy
- main/master — production-ready code
- feature/TICKET-description — new features
- fix/TICKET-description — bug fixes
- Never commit directly to main

## Before Committing
1. Run tests
2. Run linter/formatter
3. Review diff (git diff --staged)
4. Write descriptive commit message
```

---

## Rule 4: Security Basics

```markdown
---
description: Basic security rules for all code
alwaysApply: true
---

# Security Rules

## Never Do
- Hardcode secrets, API keys, passwords, or tokens
- Log sensitive data (passwords, tokens, PII)
- Use dynamic code execution functions that run arbitrary strings
- Trust user input without validation
- Commit .env files or secret files

## Always Do
- Use environment variables for secrets
- Validate and sanitize all external input
- Use parameterized queries (never string concatenation for SQL)
- Keep dependencies updated
- Add sensitive paths to .gitignore AND .cursorignore
```

---

## Rule 5: Agent Instructions

```markdown
---
description: How the AI agent should behave in this project
alwaysApply: true
---

# Agent Behavior

## Planning
- Use Plan Mode for any task with 3+ steps
- Research codebase before proposing changes
- Present approach before implementing

## Implementation
- Make changes in small, committable steps
- Follow existing patterns in the codebase
- Reuse existing utilities and helpers — don't reinvent
- Run tests after each change

## Communication
- Explain what changed and why
- Flag any risks or trade-offs
- If unsure about approach, ask before implementing

## Do NOT
- Modify files outside the requested scope
- Add features not explicitly requested
- Skip testing to save time
- Make breaking changes without warning
```

---

## Installation

```bash
# Create rules directory
mkdir -p .cursor/rules

# Copy each rule above into its own .mdc file:
# .cursor/rules/project-overview.mdc
# .cursor/rules/code-quality.mdc
# .cursor/rules/git-workflow.mdc
# .cursor/rules/security-basics.mdc
# .cursor/rules/agent-instructions.mdc
```

## When to Graduate to Intermediate

Move to the [15-rule intermediate set](intermediate-15-rules.md) when:
- You find yourself correcting the agent on the same pattern 3+ times
- Your project has framework-specific conventions (React, Terraform, etc.)
- You're working with a team and need stricter consistency
- You want domain-specific rules (DevOps, security, data science)
