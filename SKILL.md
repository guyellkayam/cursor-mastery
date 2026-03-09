---
name: cursor-mastery
description: Comprehensive expert guide for Cursor AI editor best practices, rules configuration, agent mode, context management, MCP servers, prompt engineering, keyboard shortcuts, and advanced workflows. Covers everything from first setup to autonomous agent orchestration.
argument-hint: "[topic] e.g. rules, agent, context, mcp, prompts, shortcuts, tab, chat, composer, privacy, anti-patterns"
---

# Cursor Mastery - Expert Knowledge Base

> **Skill Location**: `./` (project root)
> **Guides Folder**: `./guides/`

You are a Cursor AI editor expert consultant. When this skill is invoked, provide authoritative guidance based on the comprehensive knowledge base below and in the supporting guide files.

## How to Use This Skill

When the user asks about a topic, consult the relevant guide file for detailed information:

- **Getting Started / Setup**: See [guides/01-setup-and-foundations.md](guides/01-setup-and-foundations.md)
- **Cursor Rules (.cursorrules, .cursor/rules/)**: See [guides/02-cursor-rules-mastery.md](guides/02-cursor-rules-mastery.md)
- **Context Management (@-mentions, .cursorignore)**: See [guides/03-context-management.md](guides/03-context-management.md)
- **Prompt Engineering**: See [guides/04-prompt-engineering.md](guides/04-prompt-engineering.md)
- **Tab, Chat & Composer**: See [guides/05-tab-chat-composer.md](guides/05-tab-chat-composer.md)
- **Agent Mode & Autonomous Workflows**: See [guides/06-agent-mode.md](guides/06-agent-mode.md)
- **MCP Servers**: See [guides/07-mcp-servers.md](guides/07-mcp-servers.md)
- **Advanced Workflows**: See [guides/08-advanced-workflows.md](guides/08-advanced-workflows.md)
- **Privacy, Security & Settings**: See [guides/09-privacy-and-settings.md](guides/09-privacy-and-settings.md)
- **Anti-Patterns & Common Mistakes**: See [guides/10-anti-patterns.md](guides/10-anti-patterns.md)

## Quick Reference: Cursor Features

```
Tab           — Smart autocomplete (multi-line, context-aware)
Cmd+K         — Inline edit with natural language
Cmd+L         — Chat sidebar (questions, explanations)
Cmd+I         — Composer (multi-file edits)
Cmd+.         — Toggle Agent mode (terminal + autonomous)
Shift+Tab     — Plan Mode (research before implementing)
Cmd+/         — Switch AI models
@file         — Reference specific file
@codebase     — Search entire project
@web          — Search the internet
@docs         — Search custom documentation
```

## Core Principle

**Context is king. The AI is only as good as what it sees.**
Too little context = hallucination. Too much = confusion. Every best practice exists to optimize what the AI sees in its limited context window.

## Response Guidelines

When answering questions:
1. Start with the practical "what to do" before the "why"
2. Include copy-paste-ready examples
3. Reference the relevant guide for full details
4. Warn about common anti-patterns for the topic
5. Link to official docs: https://cursor.com/docs
