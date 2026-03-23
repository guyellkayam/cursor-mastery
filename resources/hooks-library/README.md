# Hooks Library

> **TL;DR**: Cursor hooks are automation triggers that run shell commands before/after tool calls and at session boundaries. Copy-paste scripts for security, formatting, testing, and notifications.

## What Are Hooks?

Hooks are shell commands that execute automatically in response to Cursor agent events. They enable:
- **Security gates** — Block dangerous operations before they execute
- **Auto-formatting** — Clean up code after every edit
- **Test triggering** — Run tests after code changes
- **Notifications** — Alert external systems (Jira, Gmail, Confluence) when work completes
- **Context management** — Load project state at session start, save progress at session end

## Hook Lifecycle Events

> Hooks were introduced in Cursor 1.7 (Oct 2025). Currently in beta — APIs may change.

| Event | When It Fires | Use Case |
|-------|--------------|----------|
| **beforeSubmitPrompt** | When a prompt is first submitted | Prompt validation, context injection |
| **beforeShellExecution** | Before a terminal command runs | Security gates, command blocking |
| **beforeMCPExecution** | Before an MCP tool call | Access control, rate limiting |
| **beforeReadFile** | Before reading a file | File access control |
| **afterFileEdit** | After a file is edited | Auto-format, lint, test trigger |
| **stop** | When the agent stops | Cost logging, summary, notifications |

## Configuration

Hooks are configured in `.cursor/hooks.json` (project) or `~/.cursor/hooks.json` (global). Command paths are relative to the hooks.json file location. All hooks from all config locations are merged and executed.

### Project-Level Hooks
```json
{
  "version": 1,
  "hooks": {
    "beforeShellExecution": [
      {
        "command": "bash .cursor/hooks/security-gate.sh",
        "description": "Block dangerous commands"
      }
    ],
    "afterFileEdit": [
      {
        "command": "bash .cursor/hooks/format.sh",
        "description": "Auto-format after edits"
      }
    ]
  }
}
```

### Global Hooks
```json
{
  "version": 1,
  "hooks": {
    "stop": [
      {
        "command": "bash ~/.cursor/hooks/on-stop.sh",
        "description": "Log cost and generate summary"
      }
    ]
  }
}
```

### Hook Script I/O

Hook scripts receive structured JSON on stdin (event context: tool name, file path, command, etc.) and return JSON on stdout to approve, modify, or block the action.

## Hook Files in This Library

| File | Hook Types | Scripts Included |
|------|-----------|-----------------|
| [pre-tool-use.md](pre-tool-use.md) | PreToolUse | Security gates, file protection, secret leak prevention, conventional commits |
| [post-tool-use.md](post-tool-use.md) | PostToolUse | Auto-format, test trigger, change logging, lint-on-save |
| [session-hooks.md](session-hooks.md) | SessionStart, PostStop | Context loading, progress saving, cost tracking, summary generation |
| [notification-hooks.md](notification-hooks.md) | Notification, SubagentStop | Jira ticket update, Gmail summary, Confluence page, terminal bell |

## Token Strategy

- Hooks run as shell commands **outside** the context window — they don't consume tokens
- Use hooks to automate repetitive tasks that would otherwise require agent instructions
- SessionStart hooks can load project-specific context, reducing the need for manual @-mentions
- PostStop hooks can save progress, ensuring continuity across sessions without memory overhead
