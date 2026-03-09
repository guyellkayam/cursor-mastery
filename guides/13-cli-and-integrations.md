# Guide 13: CLI & Integrations

> **Level**: 6-9
> **Prerequisites**: Guides 01-03
> **Official Docs**: https://cursor.com/docs/cli

## Cursor CLI

The Cursor CLI lets you interact with AI agents directly from your terminal — write, review, and modify code without opening the editor.

### Installation
```bash
# macOS/Linux (one-line installer)
curl -fsSL https://cursor.com/install | sh

# Or download from cursor.com/install for your platform
# Supports: macOS, Linux, WSL, Windows PowerShell
```

### Three Operating Modes

| Mode | What It Does | Flag |
|------|-------------|------|
| **Agent** | Full access to all tools (default) | `agent` |
| **Plan** | Design approach before coding, asks clarifying questions | `--plan` |
| **Ask** | Read-only exploration, no changes | `--ask` |

### Basic Usage

**Interactive Mode:**
```bash
# Start a conversation
cursor agent "refactor the auth module to use JWT tokens"

# Plan mode
cursor agent --plan "add payment processing"

# Ask mode (read-only)
cursor agent --ask "how does the auth flow work?"
```

**Non-Interactive (Scripts/CI):**
```bash
# One-shot with prompt flag
cursor agent -p "fix all TypeScript errors" --model claude-3.5-sonnet

# Headless mode for CI pipelines
cursor agent -p "run tests and fix failures" --yolo
```

### Cloud Agent Handoff
Push conversations to cloud infrastructure:
```bash
# With -c flag
cursor agent -c "refactor the entire auth system"

# With & prefix
cursor agent "& migrate database schema"

# Monitor at cursor.com/agents
```

### Session Management
```bash
# List previous sessions
cursor agent ls

# Resume a previous conversation
cursor agent resume <session-id>

# Resume most recent
cursor agent resume
```

### Shell Mode
Quick, non-interactive command execution:
- Runs commands in your `$SHELL`
- 30-second hard timeout
- Directory changes don't persist between runs
- Best for: status checks, quick builds, file operations

```bash
# Example: run in a specific directory
cursor shell "cd subdir && npm test"
```

**Limitations:**
- 30-second timeout (non-configurable)
- No long-running processes or servers
- No interactive prompts

### Sandbox Controls
Configure command execution security:
```bash
/sandbox         # Toggle security modes
/sandbox on      # Enable sandbox
/sandbox off     # Disable sandbox
```

### Max Mode
Enhanced reasoning for complex tasks:
```bash
/max-mode on     # Enable max reasoning
/max-mode off    # Disable
```

### ACP Protocol (Agent Client Protocol)
For building custom clients and integrations:
```bash
cursor agent acp   # Start ACP over stdio (JSON-RPC)
```

- Used by JetBrains integration
- JSON-RPC protocol over stdio
- Supports MCP servers
- Enables headless/CI workflows

---

## Integrations

### Slack Integration
Trigger Cloud Agents directly from Slack:
```
@Cursor [repo=github.com/owner/repo] fix the failing test
@Cursor deploy the latest changes
```

**Setup:**
1. Install Cursor Slack app
2. Set default repo in Cursor dashboard
3. Mention `@Cursor` with tasks

**Works with Linear** — bidirectional: Linear issues → Slack → implementation.

### Linear Integration
Connect project management with coding:
- Create issues in Linear
- Cursor agents pick up and implement
- Auto-link PRs to issues
- Track bugs and features

**Setup:** Link Linear workspace in Cursor dashboard settings.

### GitHub Integration
- Repository access for agents and Bugbot
- PR creation and management
- CI/CD integration (GitHub Actions)
- Bugbot auto-reviews
- Cloud Agent auto-fix for CI failures

**Required:** GitHub account linked in Cursor settings.

### GitLab Integration
- Similar to GitHub integration
- Requires **Maintainer** role
- Needs **Project Access Token**
- Note: Free tier may have limitations

### JetBrains Integration
Use Cursor's AI inside JetBrains IDEs via ACP:
```bash
cursor agent acp   # Starts ACP server for JetBrains
```

- Runs via Agent Client Protocol
- JSON-RPC over stdio
- Full agent capabilities inside IntelliJ, WebStorm, etc.

### Deeplinks
Open Cursor directly to specific files/projects:
```
cursor://file/path/to/file.ts
cursor://project/path/to/project
```

Useful for documentation, issue trackers, and CI systems.

---

## Headless / CI Mode

Run Cursor agents in CI/CD pipelines without UI:

### Basic CI Usage
```bash
# In your CI pipeline
cursor agent -p "run all tests and fix any failures" --yolo
```

### GitHub Actions Example
```yaml
- name: Cursor AI Fix
  run: |
    curl -fsSL https://cursor.com/install | sh
    cursor agent -p "fix TypeScript compilation errors" --model claude-3.5-sonnet
```

### Flags for CI
| Flag | What It Does |
|------|-------------|
| `-p` | Provide prompt (non-interactive) |
| `--model` | Select AI model |
| `--yolo` | Auto-approve tool calls |
| `-c` | Hand off to cloud agent |
| `--approve-mcps` | Auto-approve MCP servers |

### MCP in Headless Mode
MCP servers need pre-approval for headless:
```bash
# Approval file location:
~/.cursor/projects/<slug>/mcp-approvals.json
```

## Sources
- [Cursor Docs: CLI](https://cursor.com/docs/cli/overview)
- [Cursor Docs: CLI Shell Mode](https://cursor.com/docs/cli/shell-mode)
- [Cursor Docs: ACP](https://cursor.com/docs/cli/acp)
- [Cursor Blog: Bugbot Autofix](https://cursor.com/blog/bugbot-autofix)
