# Guide 06: Agent Mode & Autonomous Workflows

> **Level**: 5-6
> **Prerequisites**: Guides 01-05
> **Official Docs**: https://cursor.com/docs/agent/overview

## What Is Agent Mode?

Agent mode is Composer with superpowers. It can:
- Read and write files across your project
- Run terminal commands (npm install, tests, builds)
- Search your codebase with grep and semantic search
- Browse the web for documentation
- Create implementation plans
- Iterate until the task is complete

It's the closest thing to having a junior developer pair-programming with you.

## Enabling Agent Mode

### Toggle in Composer
1. Open Composer: `Cmd+I`
2. Toggle Agent: `Cmd+.` or press `Cmd+I` again
3. The mode indicator changes to "Agent"

### Auto-Run Configuration
**Settings > Cursor Settings > Agents > Auto-Run**:

| Mode | Behavior |
|------|----------|
| **Ask Every Time** | Prompts before each command (safest) |
| **Run Everything** (YOLO) | Runs all commands without asking |
| **Allowlist** | Auto-runs specific commands, asks for others |
| **Denylist** | Blocks specific commands, runs others |

### YOLO Mode
"Run Everything" mode — the agent executes terminal commands without asking for approval.

**When to use**: Development/testing in isolated environments.
**When NOT to use**: Production access, sensitive data, shared environments.

Configure safety rails:
```
Settings > Agents:
  Auto-Run: Allowlist
  Allowed: npm test, npm run lint, bun test, prettier
  Blocked: rm -rf, git push, docker rm
  No Delete: ✓ (prevents file deletions)
```

## Agent Workflows

### Plan Mode (Recommended for Complex Tasks)

Press `Shift+Tab` in Composer to enable Plan Mode:

```
I need to add a complete payment integration with Stripe:
- Checkout flow
- Webhook handling
- Subscription management
Research the codebase first, create a plan, and wait for approval.
```

The agent will:
1. **Research**: Search codebase for existing payment code, dependencies, patterns
2. **Plan**: Create a step-by-step implementation plan
3. **Wait**: Present the plan for your review
4. **Execute**: After approval, implement step by step

Plans are saved to `.cursor/plans/` as Markdown — reusable and shareable.

### Test-Driven Development with Agent

```
I want TDD for the UserService:
1. First, write failing tests for these scenarios:
   - Create user with valid data
   - Create user with duplicate email (should fail)
   - Get user by ID (found)
   - Get user by ID (not found)
2. Confirm the tests fail
3. Then implement UserService to pass all tests
4. Do NOT modify tests during implementation
```

### Codebase Learning

Treat the agent like a teammate who just joined:
```
Explore the project and explain:
- How is authentication implemented?
- What's the database architecture?
- Where are API endpoints defined?
- How do we handle errors?
```

### Multi-File Refactoring

```
Rename the "User" model to "Account" across the entire codebase:
- Database schema
- All TypeScript types/interfaces
- API routes
- Tests
- Documentation
Make sure nothing breaks — run tests after each change.
```

## Agent Tools

The agent has access to these built-in tools:

| Tool | What It Does |
|------|-------------|
| **File Read/Write** | Create, edit, delete files |
| **Terminal** | Run shell commands |
| **Grep/Search** | Search file contents |
| **Semantic Search** | AI-powered codebase search |
| **Browser** | Open URLs, take screenshots |
| **MCP Tools** | External integrations via MCP servers |

## Background Agents & Mission Control

### Background Agents
Agents can run in the background while you work:
- Start a task in Agent mode
- Continue working in another file
- Agent completes the task and notifies you

### Mission Control
Manage multiple agents in a grid view:
- Monitor multiple running agents
- Switch between agent tasks
- Preview changes before applying
- Each agent works in its own branch context

## Checkpoints & Safety

### Reviewing Changes
- Agent shows diffs for every file change
- Review before accepting
- **Stop** the agent if it's going in the wrong direction

### Git as Safety Net
```
# Before complex agent tasks:
git stash  # or commit current work
# Let agent work
# If something goes wrong:
git stash pop  # or git checkout .
```

### Iterative Approach
Don't let the agent do everything at once:
```
Step 1: Create the database schema [review, commit]
Step 2: Create the API layer [review, commit]
Step 3: Create the tests [review, commit]
Step 4: Add error handling [review, commit]
```

Each commit is a checkpoint you can rollback to.

## Agent Best Practices

### Do
- Start with Plan Mode for complex tasks
- Commit between agent steps
- Use specific, constrained prompts
- Let the agent search (don't @-mention everything)
- Review every diff carefully
- Use allowlist for auto-run commands

### Don't
- Give the agent unlimited access (YOLO mode in production)
- Accept all changes without reviewing
- Let long agent sessions run without checkpoints
- Ignore terminal output from agent commands
- Trust the agent blindly with destructive operations

### When Agent Mode Shines
- Setting up new projects from scratch
- Multi-file refactoring
- Adding a feature across multiple layers
- Debugging with terminal access
- Running tests and fixing failures iteratively

### When to Use Simpler Tools
- Quick one-line fix → Tab or Cmd+K
- Understanding code → Chat (Cmd+L)
- Single-file edit → Composer (non-agent)

## Sources
- [Cursor Docs: Agent](https://cursor.com/docs/agent/overview)
- [Cursor Docs: Terminal](https://cursor.com/docs/agent/terminal)
- [Cursor Blog: Agent Best Practices](https://cursor.com/blog/agent-best-practices)
