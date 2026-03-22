# Agent Mode Patterns

> **TL;DR**: How to use each Cursor agent mode effectively — Plan, Debug, YOLO, Ask, Manual, and Custom modes with real-world workflow patterns.

## Mode Overview

### Agent Mode (Default) — Cmd+.
Full autonomous capabilities: read/write files, run terminal, search codebase, browse web.

**Best for**: Feature implementation, bug fixes, refactoring

```
Prompt: "Implement user authentication with OAuth2. Use the existing
auth middleware pattern in src/middleware/auth.ts. Add tests."

Agent will:
1. Search codebase for auth patterns
2. Implement OAuth2 flow
3. Write tests
4. Run tests to verify
```

### Plan Mode — Shift+Tab
Research codebase first, create plan, wait for approval, then implement.

**Best for**: Complex multi-file changes, architectural decisions, unfamiliar codebases

```
Prompt: "Plan a migration from REST to gRPC for the user service."

Plan Mode will:
1. Analyze current REST endpoints
2. Map to gRPC service definitions
3. Identify affected files
4. Present plan for approval
5. Implement only after approval
```

### Ask Mode — /ask
Read-only. Searches and reads codebase but makes NO changes.

**Best for**: Understanding code before modifying, exploring unfamiliar repos, learning

```
Prompt: "How does the payment processing flow work? Trace from
API endpoint to database write."

Ask Mode will:
1. Search for payment-related code
2. Trace the call chain
3. Explain the flow
4. Never modify anything
```

### Manual Mode — /manual
Only edits files you explicitly select. No searching, no terminal commands.

**Best for**: Precise, targeted edits where you know exactly what to change

```
Prompt: "Change the database connection timeout from 30s to 60s"
[with the config file selected]

Manual Mode will:
1. Read only the selected file
2. Make the specific change
3. Nothing else
```

### Debug Mode
Automatically attached to errors. Reads error messages, logs, and stack traces.

**Best for**: Runtime errors, test failures, build issues

```
[Test fails with error output]
Agent automatically:
1. Reads error message and stack trace
2. Locates relevant source code
3. Identifies root cause
4. Proposes and applies fix
5. Re-runs test to verify
```

### YOLO Mode
Agent runs terminal commands without asking for confirmation.

**Best for**: Trusted environments, rapid prototyping, personal projects

**Safety Settings**:
| Setting | Description |
|---------|-------------|
| Ask Every Time | Default — confirms each command |
| YOLO | Runs everything without asking |
| Allowlist | Auto-approve specific commands (npm test, git status) |
| Denylist | Block specific commands (rm -rf, git push --force) |

**Warning**: Never use YOLO in production environments or with destructive commands.

---

## Custom Modes

Create custom modes with specific tool combinations and instructions:

```json
// .cursor/modes.json
{
  "modes": {
    "security-review": {
      "name": "Security Review",
      "description": "Read-only security analysis",
      "tools": ["search", "read"],
      "instructions": "Review code for OWASP Top 10 vulnerabilities. Report findings without making changes."
    },
    "tdd": {
      "name": "TDD Mode",
      "description": "Test-Driven Development",
      "tools": ["search", "read", "write", "terminal"],
      "instructions": "Always write failing test first, then implement code to pass it."
    },
    "docs-only": {
      "name": "Documentation",
      "description": "Only create/edit documentation files",
      "tools": ["search", "read", "write"],
      "instructions": "Only modify .md files, README, and inline comments. Never change source code."
    }
  }
}
```

---

## Workflow Patterns

### Pattern 1: Explore → Plan → Implement → Verify
```
1. /ask "How does the auth system work?" (Ask mode — understand)
2. Shift+Tab "Plan migration to JWT tokens" (Plan mode — design)
3. Cmd+. "Implement the JWT migration plan" (Agent mode — build)
4. "Run all auth tests and verify" (Agent mode — verify)
```

### Pattern 2: TDD with Agent
```
1. "Write a failing test for [feature]" (Agent writes test)
2. "Run the test — confirm it fails" (Agent runs test)
3. "Implement the minimum code to pass" (Agent implements)
4. "Refactor while keeping tests green" (Agent refactors)
```

### Pattern 3: Multi-Model Comparison
```
1. Ask Claude Sonnet: "How would you implement [feature]?"
2. Switch model (Cmd+/): Ask GPT-4o same question
3. Switch model: Ask Gemini 2.5 Pro same question
4. Compare approaches, pick best elements from each
```

### Pattern 4: Parallel Subagent Research
```
Main agent: "I need to understand 3 things before implementing:
- Agent 1: Search for existing auth patterns
- Agent 2: Check all API endpoints that need auth
- Agent 3: Review the current test coverage

Launch each as a background agent, then synthesize findings."
```

---

## Queued Messaging (Async Task Stacking)

While the agent is working, you can queue follow-up instructions:
- **Enter** — Queue task sequentially (runs after current task completes)
- **Cmd+Enter** — Interrupt with immediate priority message

This enables asynchronous multi-step workflows without waiting.

---

## Real-World Case Studies

### Money Forward (1,000+ employees)
- Engineers saved **15-20 hours weekly**
- QA reduced test case generation by **70%** using Jira → Playwright agent pipeline
- Product managers extracted system relationships from repos → grounded PRDs in actual code
- Designers accessed analytics + frontend code → eliminated mockup-reality gap

### PlanetScale (Bugbot)
- Autonomous vulnerability detection and fixing in CI pipeline
- Agent scans code → generates fix → runs tests → submits PR → human reviews

---

## Key Insight: Rules vs Skills

| Aspect | Rules (.cursor/rules/) | Skills (SKILL.md) |
|--------|----------------------|-------------------|
| Loading | Included in every conversation | Loaded only when contextually relevant |
| Best for | Build commands, conventions, guardrails | Specialized workflows, domain knowledge |
| When to add | When agent makes same mistake 3+ times | When you have a repeatable workflow |
| Size | Keep under 500 lines each | Can be larger (loaded on demand) |

---

## Token Strategy

- **Ask first, Agent second**: Use Ask mode to understand (~50% fewer tokens than exploring in Agent mode)
- **Plan mode**: Creates a focused plan — implementation only touches planned files
- **Custom modes**: Restrict tools to only what's needed — fewer tools = fewer tokens
- **New conversation per task**: Fresh context window avoids paying for stale history
- **Queued messaging**: Stack follow-ups instead of starting new conversations
