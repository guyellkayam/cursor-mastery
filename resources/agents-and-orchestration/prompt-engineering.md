# Prompt Engineering & Workflow Orchestration

> **TL;DR**: Boris Cherny's 7 workflow principles adapted as Cursor .mdc rules, plus team-of-agents prompts and structured instruction patterns.

## Boris Cherny's 7 Principles — Adapted for Cursor

These principles originally defined in a CLAUDE.md file are adapted here as `.cursor/rules/*.mdc` rules for Cursor AI.

---

### Principle 1: Plan Mode Default

> Use Plan Mode (Shift+Tab) for ANY task with 3+ steps or architectural decisions.

```markdown
---
description: Enforce Plan Mode for complex tasks
alwaysApply: true
---

# Plan Mode Default

Before implementing ANY task that involves:
- 3 or more steps
- Architectural decisions
- Multi-file changes
- New feature additions
- Refactoring existing code

ALWAYS:
1. Switch to Plan Mode (Shift+Tab)
2. Research the codebase first
3. Present a plan with file paths and changes
4. Wait for approval before implementing
5. Implement in discrete, committable steps

Never skip planning for non-trivial work. The cost of planning is always less than the cost of rework.
```

---

### Principle 2: Subagent Strategy

> Spin up background agents (up to 8) for research, exploration, and analysis.

```markdown
---
description: Use subagents for parallel work
alwaysApply: true
---

# Subagent Strategy

When facing tasks that involve:
- Searching multiple areas of the codebase
- Comparing implementation approaches
- Running tests while writing code
- Generating docs while implementing

Delegate to background agents:
- Use up to 8 parallel subagents via git worktrees
- Each subagent gets isolated context (no cross-contamination)
- One agent per task — don't overload a single agent
- Let main agent focus on architecture decisions

Pattern:
- Agent 1: Research existing patterns
- Agent 2: Write tests
- Agent 3: Implement feature
- Agent 4: Update documentation
```

---

### Principle 3: Self-Improvement Loop

> After ANY correction, capture the pattern as a new .mdc rule.

```markdown
---
description: Learn from corrections by creating rules
alwaysApply: true
---

# Self-Improvement Loop

When you receive a correction or learn a project-specific pattern:

1. Fix the immediate issue
2. Create a NEW `.cursor/rules/` MDC rule capturing the pattern:
   - Use descriptive kebab-case filename
   - Add appropriate `globs` if file-type specific
   - Write clear instructions in markdown body
3. Reference the new rule in your response

Example: If corrected about import ordering →
Create `.cursor/rules/import-ordering.mdc`:
```yaml
---
description: Enforce project import ordering convention
globs: ["*.ts", "*.tsx"]
---
# Import Ordering
1. Node built-ins (node:path, node:fs)
2. External packages (react, next)
3. Internal packages (@/lib, @/utils)
4. Relative imports (./component)
Blank line between each group.
```

This ensures the same mistake never happens twice.
```

---

### Principle 4: Verification Before Done

> Never mark a task complete without proving it works.

```markdown
---
description: Require verification before task completion
alwaysApply: true
---

# Verification Before Done

Before declaring ANY task complete:

1. **Run the code** — Build succeeds, no errors
2. **Run tests** — All tests pass (existing + new)
3. **Manual check** — If UI change, verify visually
4. **Lint/format** — No style violations
5. **Type check** — No TypeScript/type errors

If ANY verification fails:
- Fix the issue
- Re-run ALL verifications
- Only then mark complete

Never say "done" or "complete" without verification output.
```

---

### Principle 5: Demand Elegance

> For non-trivial changes, pause and ask "is there a more elegant way?"

```markdown
---
description: Seek elegant solutions for non-trivial changes
alwaysApply: true
---

# Demand Elegance

For ANY change that is non-trivial (3+ lines, new function, architectural):

1. Write the first working solution
2. PAUSE — before presenting it, ask yourself:
   - Is there a simpler approach?
   - Can I reuse existing code/patterns?
   - Will this be easy to maintain?
   - Is the naming clear and consistent?
3. If a better approach exists, refactor before presenting
4. For complex solutions, switch to Ask mode to evaluate alternatives

Prefer:
- 3 simple lines over 1 clever line
- Existing patterns over new abstractions
- Standard library over custom implementations
- Composition over inheritance
```

---

### Principle 6: Autonomous Bug Fixing

> Given a bug report, fix it without asking for hand-holding.

```markdown
---
description: Fix bugs autonomously using Agent + Debug mode
alwaysApply: true
---

# Autonomous Bug Fixing

When given a bug report:

1. **Reproduce** — Find the failing test or create one
2. **Locate** — Search codebase for the root cause (don't ask where to look)
3. **Understand** — Read surrounding code to understand intent
4. **Fix** — Implement the minimal fix
5. **Verify** — Run tests, confirm the bug is fixed
6. **Prevent** — Add a test that would catch regression

Use Agent Mode with Debug capabilities:
- Read error messages carefully
- Check logs and stack traces
- Use @codebase to find related code
- Don't ask "where should I look?" — search autonomously

Only ask for clarification if:
- The bug report is ambiguous about expected behavior
- Multiple valid fixes exist with different tradeoffs
```

---

### Principle 7: Task Management

> Plan → Verify Plan → Track Progress → Explain Changes → Document Results → Capture Lessons

```markdown
---
description: Structured task management workflow
alwaysApply: true
---

# Task Management Workflow

For every task, follow this 6-step workflow:

## 1. Plan
- Switch to Plan Mode
- Research codebase
- List files to modify
- Describe approach

## 2. Verify Plan
- Check for edge cases
- Identify risks
- Confirm approach aligns with project patterns

## 3. Track Progress
- Use checkpoints after each significant change
- Commit between major steps
- Keep conversation focused (one task per conversation)

## 4. Explain Changes
- Describe what changed and why
- Highlight any non-obvious decisions

## 5. Document Results
- Update relevant docs if behavior changed
- Add/update comments only where logic isn't self-evident

## 6. Capture Lessons
- If anything surprising happened, create a new .cursor/rules/ MDC rule
- Update AGENTS.md if architectural patterns were established
```

---

## Team-of-Agents Prompts

### Research Team (3 agents)
```
Agent 1 (Explorer): "Search the entire codebase for [pattern]. List every file that
matches, with line numbers and context. Do NOT modify any files."

Agent 2 (Analyst): "Read [file1, file2, file3] and explain: (a) the current architecture,
(b) data flow between components, (c) any technical debt or inconsistencies."

Agent 3 (Recommender): "Given [requirements], search for existing implementations in
the codebase that solve similar problems. Recommend reuse vs. new implementation."
```

### Implementation Team (4 agents)
```
Agent 1 (Test Writer): "Write failing tests for [feature] based on [spec]. Use existing
test patterns from [test-dir/]. Do NOT implement the feature."

Agent 2 (Implementer): "Implement [feature] to make all tests in [test-file] pass.
Follow patterns in [reference-file]. Run tests after each change."

Agent 3 (Documenter): "Update documentation for [feature]: API docs, README section,
inline comments where logic isn't obvious. Read the implementation first."

Agent 4 (Reviewer): "Review the diff for [feature]. Check: security, performance,
error handling, naming, test coverage. List issues by severity."
```

### DevOps Team (3 agents)
```
Agent 1 (IaC): "Generate Terraform module for [resource] following the patterns in
terraform/modules/. Include variables.tf, outputs.tf, and a README."

Agent 2 (CI/CD): "Create GitHub Actions workflow for [task] following the patterns in
.github/workflows/. Include matrix strategy and caching."

Agent 3 (Security): "Scan [directory] for security issues: secrets in code,
misconfigured permissions, vulnerable dependencies. Use Trivy patterns."
```

---

## Structured Instruction Pattern

When giving Cursor agent complex instructions, use this template:

```
## Task
[One sentence: what to do]

## Context
[2-3 sentences: why this matters, what exists currently]

## Requirements
- [ ] Requirement 1
- [ ] Requirement 2
- [ ] Requirement 3

## Constraints
- Must follow patterns in [reference file]
- Must not modify [protected files]
- Must pass all existing tests

## Acceptance Criteria
- [ ] All tests pass
- [ ] No linting errors
- [ ] Changes committed with descriptive message

## References
@file1 @file2 @codebase
```

---

## Token Strategy

- **Rule scoping via globs**: Each .mdc rule uses `globs` to only load when relevant files are open — saves tokens
- **One principle per rule**: Don't combine all 7 principles into one massive rule — let Cursor auto-attach only what's relevant
- **Subagent delegation**: Complex analysis → background agent with isolated context window
- **Ask mode for exploration**: Use Ask mode (read-only) before Agent mode (read-write) to understand before acting
- **40-tool MCP limit**: Only configure MCP servers you actively use — Cursor sends only first 40 tools to agent
