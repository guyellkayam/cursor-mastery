# Composer Workflows

> **TL;DR**: Multi-file creation and editing with visual diffs — checkpoints, linting, iterative refinement, and when to use Composer vs Agent mode.

## What Is Composer?

Composer (Cmd+I) is Cursor's multi-file editing interface:
- Creates and edits multiple files simultaneously
- Shows visual diffs for every change
- Creates checkpoints for easy rollback
- Auto-fixes linting issues
- Supports all context features (@-mentions)

## When to Use Composer vs Agent

| Use Composer When | Use Agent When |
|-------------------|---------------|
| Creating multiple new files | Need terminal commands |
| Visual review of diffs is important | Need codebase search |
| Precise multi-file edits | Need iterative debugging |
| Want checkpoint/rollback ability | Need web browsing |
| Working on known files | Exploring unfamiliar code |

## Core Workflows

### Workflow 1: Multi-File Feature Creation
```
Cmd+I → "Create a new API endpoint for user profiles:
- src/routes/profile.ts — route handler
- src/models/profile.ts — data model
- src/services/profile.ts — business logic
- tests/profile.test.ts — tests

Follow patterns from @file:src/routes/user.ts"

Composer creates all 4 files with visual diffs.
Review each diff, accept or modify.
```

### Workflow 2: Cross-File Refactoring
```
Cmd+I → "Rename the 'getUserData' function to 'fetchUserProfile'
across all files. Update imports, function calls, and test references.
@codebase search for all occurrences first."

Composer shows diffs for every affected file.
Review changes file by file, accept all or selectively.
```

### Workflow 3: Iterative Refinement
```
Round 1: "Create a React component for the settings page"
[Review diff, accept]

Round 2: "Add dark mode toggle to the settings component"
[Review diff, accept]

Round 3: "Add form validation for the email field"
[Review diff, accept]

Each round builds on the previous — checkpoints let you roll back any step.
```

## Checkpoints

Every Composer generation creates a **checkpoint**:
- Automatically saved before each change
- Revert to any previous checkpoint
- Compare current state to any checkpoint
- No manual save needed

### Checkpoint Strategy
```
1. Accept initial generation → Checkpoint 1
2. Refine with additional prompt → Checkpoint 2
3. Something goes wrong → Revert to Checkpoint 1
4. Try different approach → Checkpoint 3
```

## Linting Integration

Composer auto-detects and fixes linting issues:
- TypeScript type errors
- ESLint violations
- Prettier formatting
- Python flake8/ruff issues

When a lint error is detected after a change, Composer offers to auto-fix.

## Context Management in Composer

### @-Mentions
```
@file:src/types.ts — reference specific file
@folder:src/components — reference directory
@codebase — search entire project
@web — include web search results
@docs — reference documentation
@git — recent changes and history
```

### Best Practices
- Reference specific files with @file for precise context
- Use @codebase for understanding patterns before editing
- Don't over-mention — each @file adds to token cost
- Let Composer search when it needs to — don't pre-load everything

## Agent Toggle in Composer

Composer has an **Agent toggle** that enables full agent capabilities within the Composer interface:
- When OFF: Composer only edits files (visual diff mode)
- When ON: Composer can also run terminal commands, search web, use MCP tools

Toggle ON when you need:
- Build verification after changes
- Test execution after edits
- Package installation during setup

Toggle OFF when you want:
- Pure file editing with visual review
- Maximum control over changes
- Faster response (less tool overhead)

---

## Token Strategy

- Composer shows full diffs — review locally instead of asking the agent to explain
- Use checkpoints instead of asking agent to "undo" (checkpoint = free, undo conversation = tokens)
- @file references are more token-efficient than pasting code into the prompt
- Agent toggle OFF = fewer tools loaded = lower token overhead per request
