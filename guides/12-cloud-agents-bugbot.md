# Guide 12: Cloud Agents & Bugbot

> **Level**: 8-10
> **Prerequisites**: Guides 01-06
> **Official Docs**: https://cursor.com/docs/cloud-agents

## Cloud Agents

Cloud Agents run in isolated virtual machines in the cloud. They clone your repo, create branches, work autonomously, and open pull requests — all without using your local machine.

### What Cloud Agents Can Do
- Clone repositories and create feature branches
- Read, write, and edit code across the entire project
- Run terminal commands (builds, tests, migrations)
- Use a full desktop environment (mouse, keyboard, browser)
- Generate artifacts (screenshots, videos, logs) attached to PRs
- Auto-fix CI failures on their PRs
- Access MCP servers for external tools

### Setup Methods

**Method 1: Agent-Driven Setup (Recommended)**
1. Visit https://cursor.com/onboard
2. The agent configures its own environment
3. Optionally create a VM snapshot for reuse

**Method 2: Manual Dockerfile (Advanced)**
Create a Dockerfile for custom environments:
```dockerfile
FROM ubuntu:22.04

RUN apt-get update && apt-get install -y \
  nodejs npm python3 git curl

# Do NOT COPY the full project
# Cursor manages the workspace and checks out the correct commit
```

**Method 3: Environment JSON**
Create `.cursor/environment.json` in your repo:
```json
{
  "install": "npm install",
  "start": "npm run dev"
}
```

### Environment Priority
1. Repository `.cursor/environment.json` (highest)
2. Personal settings
3. Team configuration

### Secrets Management
- Managed via Cursor dashboard > Secrets tab
- KMS encryption at rest
- Exposed as environment variables to cloud agents
- Optional redaction to prevent accidental commits

### Triggering Cloud Agents

**From CLI:**
```bash
# Start a task
cursor agent "fix the login redirect bug"

# Hand off to cloud with -c flag
cursor agent -c "refactor auth module to use JWT"

# Or prefix with &
cursor agent "& migrate database to Prisma 5"
```

**From Slack:**
```
@Cursor [repo=github.com/owner/repo] fix the failing test in auth.test.ts
```

**From Web:**
Visit https://cursor.com/agents to monitor and manage running agents.

### CI Failure Auto-Fix
Cloud agents automatically attempt to fix CI failures in their PRs:
- Currently supports GitHub Actions
- Enabled by default for team accounts
- Disable globally in dashboard settings
- Disable per-PR: comment `@cursor autofix off`

### Computer Use
Cloud agents have full desktop environments:
- Start dev servers and open browsers
- Take screenshots for visual verification
- Interact with UIs for testing
- Validate changes before submitting PRs

### Limitations
- Limited memory/CPU on default VMs (Enterprise can request more)
- Computer use unavailable with custom Dockerfiles/snapshots
- Docker inside VM requires fuse-overlayfs configuration

---

## Bugbot (Automated PR Review)

Bugbot is Cursor's automated pull request reviewer. It scans PRs for bugs, suggests fixes, and can auto-fix issues via Cloud Agents.

### What Bugbot Does
- Reviews every PR automatically
- Identifies potential bugs and issues
- Provides inline code review comments
- **Autofix**: Spawns Cloud Agents to fix issues it finds
- Attaches screenshots and artifacts to PRs

### Stats
- Reviews 2M+ PRs per month
- 76% issue resolution rate
- 35%+ of Autofix changes merge successfully

### Setup
1. Go to https://cursor.com (dashboard)
2. Link your GitHub organization/repos
3. Bugbot auto-runs on every PR update

### How It Works
1. PR is created or updated
2. Bugbot analyzes all changed files
3. Posts inline review comments for issues found
4. **Autofix** (if enabled): Spawns a Cloud Agent in a VM
5. Cloud Agent attempts to fix the identified issues
6. Fixed code is pushed as a new commit to the PR

### Configuration
- Enable/disable per-repo in dashboard
- Disable per-PR: comment `@cursor autofix off`
- Team accounts: enabled by default

### Roadmap
- Code verification
- Deep research capabilities
- Continuous codebase scanning

---

## Parallel Agents

Run up to 8 agents concurrently from a single prompt.

### How It Works
- Each agent gets an isolated git worktree (or remote machine)
- Agents work independently — no conflicts
- Results aggregated as multi-file diffs
- Review all changes in one place

### Use Cases
```
Run these 3 tasks in parallel:
1. Add unit tests for the auth module
2. Refactor the payment service error handling
3. Update the API documentation

Use separate agents for each task.
```

### How to Trigger
- Describe multiple independent tasks in one prompt
- Agent automatically spawns parallel workers
- Monitor via Mission Control grid view

### Best Practice
- Tasks must be independent (no file conflicts)
- Each parallel agent = separate token usage
- Review carefully — parallel changes may need reconciliation

## Sources
- [Cursor Docs: Cloud Agents](https://cursor.com/docs/cloud-agents)
- [Cursor Blog: Bugbot Autofix](https://cursor.com/blog/bugbot-autofix)
- [Cursor Blog: Building Bugbot](https://cursor.com/blog/building-bugbot)
