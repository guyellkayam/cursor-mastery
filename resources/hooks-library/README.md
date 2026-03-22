# Hooks Library

> **TL;DR**: Cursor hooks are automation triggers that run shell commands before/after tool calls and at session boundaries. Copy-paste scripts for security, formatting, testing, and notifications.

## What Are Hooks?

Hooks are shell commands that execute automatically in response to Cursor agent events. They enable:
- **Security gates** — Block dangerous operations before they execute
- **Auto-formatting** — Clean up code after every edit
- **Test triggering** — Run tests after code changes
- **Notifications** — Alert external systems (Jira, Gmail, Confluence) when work completes
- **Context management** — Load project state at session start, save progress at session end

## Hook Types

| Type | When it Fires | Use Case |
|------|--------------|----------|
| **PreToolUse** | Before any tool call (file edit, terminal command, etc.) | Security gates, file protection, conventional commits |
| **PostToolUse** | After a tool call completes | Auto-format, test trigger, change logging |
| **SessionStart** | When a new Cursor session begins | Load context, restore state, check environment |
| **PostStop** | When a session ends or agent stops | Save progress, cost logging, summary generation |
| **SubagentStop** | When a background agent completes | Aggregate results, trigger next phase |
| **Notification** | When agent wants to notify the user | External alerts: email, Jira, Telegram |

## Configuration

Hooks are configured in your Cursor settings or project configuration:

### Project-Level Hooks
```json
// .cursor/hooks.json (or equivalent project config)
{
  "hooks": {
    "PreToolUse": [
      {
        "command": "bash .cursor/hooks/pre-tool-use.sh",
        "description": "Security gate: block writes to protected files"
      }
    ],
    "PostToolUse": [
      {
        "command": "bash .cursor/hooks/post-tool-use.sh",
        "description": "Auto-format after edits"
      }
    ]
  }
}
```

### Global Hooks
```json
// ~/.cursor/hooks.json
{
  "hooks": {
    "SessionStart": [
      {
        "command": "bash ~/.cursor/hooks/session-start.sh",
        "description": "Load global context on every session"
      }
    ]
  }
}
```

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
