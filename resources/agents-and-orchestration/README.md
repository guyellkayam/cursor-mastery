# Agents & Orchestration

> **TL;DR**: Everything about Cursor agent capabilities — modes, subagents, cloud agents, Composer workflows, prompt engineering, and AGENTS.md templates.

## Files in This Section

| File | What's Inside |
|------|--------------|
| [Agent Mode Patterns](agent-mode-patterns.md) | Plan, Debug, YOLO, Ask, Manual, Custom modes |
| [Subagent Configurations](subagent-configurations.md) | 8 parallel agents, git worktrees, specialized roles |
| [Cloud Agents](cloud-agents.md) | Cloud handoff, VMs, computer use, CI auto-fix |
| [Composer Workflows](composer-workflows.md) | Multi-file editing, checkpoints, refinement |
| [Prompt Engineering](prompt-engineering.md) | Boris Cherny 7 principles, team-of-agents prompts |
| [AGENTS.md Templates](agents-md-templates.md) | Ready-to-use AGENTS.md files for different project types |

## Quick Reference

| Mode | Shortcut | When to Use |
|------|----------|-------------|
| Agent | Cmd+. | Full autonomous mode — search, edit, run commands |
| Plan | Shift+Tab | Research first, plan, then implement (3+ step tasks) |
| Ask | /ask | Read-only exploration — understand before changing |
| Manual | /manual | Precise control — only edit specified files |
| Composer | Cmd+I | Multi-file creation and editing with visual diff |
| Debug | Built-in | Attached to errors, reads logs, fixes autonomously |

## Token Strategy

- **Ask mode first**: Explore codebase (read-only) before Agent mode (read-write) — avoids wasted edits
- **New conversation per task**: Don't accumulate context from previous tasks
- **Subagent delegation**: Complex research → background agent with isolated context
- **Plan mode**: Front-loads thinking, reduces back-and-forth tokens
