# Cloud Agents

> **TL;DR**: Run Cursor agents in cloud VMs — cloud handoff with `&`, isolated environments, computer use capabilities, CI auto-fix, and parallel execution.

## What Are Cloud Agents?

Cloud agents run in **isolated virtual machines** in the cloud:
- Full development environment (OS, tools, packages)
- No laptop connection required
- Multiple agents can run simultaneously
- Each agent gets its own VM
- Computer use capabilities (browser, desktop)

## Cloud Handoff

### Quick Start: `&` Prefix
Prepend `&` to any message to send it to a cloud agent:

```
& Refactor the authentication module to use JWT tokens.
Add tests and update the API documentation.
```

The agent runs in the cloud while you continue working locally.

### CLI Cloud Flag
```bash
cursor agent -c "Implement feature X and create a PR"
```

### Pick Up Later
- Cloud agents continue running even if you close Cursor
- Check progress at cursor.com/agents
- Resume conversation from web or mobile

## Environment Setup

### Agent-Driven (Recommended)
Let the cloud agent set up its own environment:
```
& Clone the repo, install dependencies, and implement [feature].
Use Node.js 20 and PostgreSQL 16.
```

### Manual Dockerfile
```dockerfile
# .cursor/Dockerfile
FROM node:20-alpine

WORKDIR /app
RUN apk add --no-cache git python3 make g++

COPY package*.json ./
RUN npm ci

COPY . .

# Install dev tools
RUN npm install -g typescript ts-node
```

### Environment JSON
```json
// .cursor/environment.json
{
  "image": "node:20",
  "setup": [
    "npm ci",
    "npx prisma generate"
  ],
  "secrets": {
    "DATABASE_URL": "env:DATABASE_URL",
    "API_KEY": "env:API_KEY"
  }
}
```

## Computer Use

Cloud agents can interact with a virtual desktop:
- **Browser**: Open URLs, fill forms, take screenshots
- **Desktop**: Run GUI applications
- **Terminal**: Full shell access
- **Artifacts**: Generate videos, screenshots, logs

### Use Cases
```
& Test the signup flow in a real browser:
1. Open localhost:3000/signup
2. Fill in the registration form
3. Submit and verify redirect to dashboard
4. Screenshot each step
```

## CI Auto-Fix

Cloud agents can automatically fix CI failures:

### Setup
```yaml
# .github/workflows/cursor-autofix.yml
name: Cursor Auto-Fix
on:
  check_suite:
    types: [completed]

jobs:
  autofix:
    if: github.event.check_suite.conclusion == 'failure'
    runs-on: ubuntu-latest
    steps:
      - name: Trigger Cursor Cloud Agent
        run: |
          curl -X POST https://api.cursor.com/agents/trigger \
            -H "Authorization: Bearer ${{ secrets.CURSOR_TOKEN }}" \
            -d '{"repo": "${{ github.repository }}", "ref": "${{ github.sha }}", "task": "Fix CI failures"}'
```

### How It Works
1. CI pipeline fails
2. Cursor cloud agent is triggered
3. Agent reads CI logs, identifies failure
4. Fixes the code
5. Creates a PR with the fix
6. Notifies team

## Parallel Cloud Agents

Run multiple cloud agents simultaneously for large tasks:

```
& Agent 1: Implement the user service endpoints
& Agent 2: Implement the order service endpoints
& Agent 3: Write integration tests for both services
& Agent 4: Set up the CI/CD pipeline
```

Each agent gets its own VM — no conflicts, full parallel execution.

## Limitations

- **Network**: Cloud VMs may have restricted network access
- **State**: VMs are ephemeral — state doesn't persist between sessions
- **Cost**: Cloud agents consume compute resources (check your plan limits)
- **Secrets**: Use environment.json for secrets — don't hardcode in prompts
- **Repo size**: Large repos take longer to clone

## GitHub Integration

Cloud agents can create PRs directly:

```
& Implement [feature], write tests, and create a PR with:
- Title: "feat: add user authentication"
- Description: Summary of changes
- Labels: enhancement
- Reviewers: @teammate
```

---

## "Use this when..."

| Scenario | Use Cloud Agent? |
|----------|-----------------|
| Long-running task (30+ minutes) | Yes — don't block your laptop |
| Need browser testing | Yes — computer use capabilities |
| CI failure auto-fix | Yes — automated pipeline repair |
| Multiple parallel tasks | Yes — each gets own VM |
| Quick file edit | No — use local agent |
| Sensitive code (can't leave machine) | No — use local agent |

## Token Strategy

- Cloud agents have their own context — no impact on local agent tokens
- Use cloud for heavy research tasks — offload token cost from your local session
- Cloud agent results come back as a PR or summary — compressed format
- Parallel cloud agents = parallel token spend (but faster overall)
