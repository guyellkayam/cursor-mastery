# Cursor AI Mastery

A comprehensive knowledge base for mastering [Cursor](https://cursor.com) — the AI-native code editor — from first setup to autonomous agent orchestration.

## What's Inside

**10 in-depth guides** covering every Cursor feature with practical, copy-paste-ready examples.

| # | Guide | What You'll Learn |
|---|-------|-------------------|
| 01 | [Setup & Foundations](guides/01-setup-and-foundations.md) | Installation, first session, core features, settings |
| 02 | [Cursor Rules Mastery](guides/02-cursor-rules-mastery.md) | .cursorrules, .cursor/rules/ MDC format, rule types |
| 03 | [Context Management](guides/03-context-management.md) | @-mentions, .cursorignore, indexing, context strategies |
| 04 | [Prompt Engineering](guides/04-prompt-engineering.md) | 5 prompt patterns, quality ladder, agent prompting |
| 05 | [Tab, Chat & Composer](guides/05-tab-chat-composer.md) | Deep dive into each AI mode, when to use what |
| 06 | [Agent Mode](guides/06-agent-mode.md) | Autonomous workflows, YOLO mode, Plan Mode, safety |
| 07 | [MCP Servers](guides/07-mcp-servers.md) | External integrations, mcp.json format, popular servers |
| 08 | [Advanced Workflows](guides/08-advanced-workflows.md) | TDD, Plan-Implement-Verify, multi-model, git workflows |
| 09 | [Privacy & Settings](guides/09-privacy-and-settings.md) | Privacy mode, model config, keyboard shortcuts, plans |
| 10 | [Anti-Patterns](guides/10-anti-patterns.md) | Common mistakes and the 10 golden rules |

## Quick Start

### Core AI Features
| Feature | Shortcut | Use Case |
|---------|----------|----------|
| **Tab** | `Tab` | Smart autocomplete — multi-line, context-aware |
| **Inline Edit** | `Cmd+K` | Edit selected code with natural language |
| **Chat** | `Cmd+L` | Ask questions, get explanations (read-only) |
| **Composer** | `Cmd+I` | Multi-file creation and editing |
| **Agent** | `Cmd+.` | Autonomous mode — terminal, browser, multi-file |
| **Plan Mode** | `Shift+Tab` | Research codebase, create plan, then implement |

### Core Principle

> **Context is king. The AI is only as good as what it sees.**
> Too little context = hallucination. Too much = confusion. Every best practice optimizes what the AI sees in its limited context window.

## Key Concepts

### Cursor Rules (.cursor/rules/)
MDC files with YAML frontmatter that inject context-specific instructions:
```markdown
---
description: TypeScript conventions for all TS files
globs: **/*.ts, **/*.tsx
alwaysApply: false
---
# Use strict mode, never use `any`, prefer interfaces...
```

### @-Mentions (Context Control)
| Mention | What It Does |
|---------|-------------|
| `@file` | Include specific file |
| `@codebase` | Search entire indexed project |
| `@web` | Search the internet |
| `@docs` | Custom documentation sources |
| `@git` | Git history and changes |

### MCP Servers (External Tools)
Connect databases, GitHub, APIs via `.cursor/mcp.json`:
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": { "GITHUB_PERSONAL_ACCESS_TOKEN": "${env:GITHUB_TOKEN}" }
    }
  }
}
```

## The 10 Golden Rules

1. Review every diff before accepting
2. Commit between agent steps for safe rollback
3. Start fresh conversations per task
4. Be specific in prompts — WHAT, WHERE, HOW
5. Use Plan Mode for complex tasks
6. Protect secrets with .cursorignore
7. Start simple — add rules only when needed
8. Let the agent search — don't over-specify context
9. Use the right tool — Tab, Cmd+K, Chat, Composer, Agent
10. Treat AI as a collaborator — guide it, review it, iterate

## Research Sources

- [Cursor Official Docs](https://cursor.com/docs)
- [Cursor Blog: Agent Best Practices](https://cursor.com/blog/agent-best-practices)
- [cursor.directory](https://cursor.directory) — 73K+ community rules
- [awesome-cursor-rules-mdc](https://github.com/sanjeed5/awesome-cursor-rules-mdc) — Curated MDC collection
- [digitalchild/cursor-best-practices](https://github.com/digitalchild/cursor-best-practices) — Battle-tested configs
- [instructa/ai-prompts](https://github.com/instructa/ai-prompts) — Curated AI prompts for Cursor
- [Cursor Forum](https://forum.cursor.com) — Community discussions and tips
- [Codecademy: How to Use Cursor AI](https://www.codecademy.com/article/how-to-use-cursor-ai)
- [Builder.io: Cursor Tips](https://www.builder.io/blog/cursor-ai-tips-react-nextjs)
- [Prismic: Cursor AI Guide](https://prismic.io/blog/cursor-ai)

## License

MIT
