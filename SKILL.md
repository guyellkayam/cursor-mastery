---
name: cursor-mastery
description: Comprehensive expert guide for Cursor AI editor best practices, rules configuration, agent mode, context management, MCP servers, prompt engineering, keyboard shortcuts, advanced workflows, skills, subagents, hooks, cloud agents, Bugbot, CLI, and integrations. Covers everything from first setup to autonomous agent orchestration.
argument-hint: "[topic] e.g. rules, agent, context, mcp, prompts, shortcuts, tab, chat, composer, privacy, anti-patterns, skills, subagents, hooks, cloud-agents, bugbot, cli, integrations"
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
- **Skills, Subagents & Hooks**: See [guides/11-skills-subagents-hooks.md](guides/11-skills-subagents-hooks.md)
- **Cloud Agents & Bugbot**: See [guides/12-cloud-agents-bugbot.md](guides/12-cloud-agents-bugbot.md)
- **CLI & Integrations**: See [guides/13-cli-and-integrations.md](guides/13-cli-and-integrations.md)

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

## Resources (Domain-Specific)

When the user asks about a specific domain, consult the resources folder:

- **Rules / .mdc files**: See [resources/rules-library/](resources/rules-library/)
- **Marketplace Plugins**: See [resources/marketplace-plugins/](resources/marketplace-plugins/)
- **MCP Servers**: See [resources/mcp-servers/](resources/mcp-servers/)
- **DevOps (Terraform, k8s, CI/CD, GitOps)**: See [resources/devops/](resources/devops/)
- **Finance (equity research, FinOps)**: See [resources/finance/](resources/finance/)
- **Security (OWASP, scanning, compliance)**: See [resources/security/](resources/security/)
- **Python (FastAPI, Django, data science)**: See [resources/python/](resources/python/)
- **Project Management (Linear, Jira)**: See [resources/project-management/](resources/project-management/)
- **Document Creation (PRD, runbooks, API docs)**: See [resources/document-creation/](resources/document-creation/)
- **Agent Orchestration (modes, subagents, cloud)**: See [resources/agents-and-orchestration/](resources/agents-and-orchestration/)
- **Hooks (pre/post tool use, session)**: See [resources/hooks-library/](resources/hooks-library/)
- **Cursor Rules Templates (starter kits)**: See [resources/cursor-rules-templates/](resources/cursor-rules-templates/)
- **AI & ML (model selection, local LLMs)**: See [resources/ai-and-ml/](resources/ai-and-ml/)
- **Data Analysis (Databricks, Snowflake)**: See [resources/data-analysis/](resources/data-analysis/)
- **Cloud & Infra (AWS, Vercel, Cloudflare)**: See [resources/cloud-and-infra/](resources/cloud-and-infra/)
- **Git & GitHub**: See [resources/git-and-github/](resources/git-and-github/)
- **Databases (PostgreSQL, Redis)**: See [resources/databases/](resources/databases/)
- **Community (awesome-cursorrules, forum)**: See [resources/community-index/](resources/community-index/)
- **Top Picks / Daily Cheatsheet**: See [resources/curated-top-picks.md](resources/curated-top-picks.md)

## Response Guidelines

When answering questions:
1. Start with the practical "what to do" before the "why"
2. Include copy-paste-ready examples
3. Reference the relevant guide for full details
4. Warn about common anti-patterns for the topic
5. Link to official docs: https://cursor.com/docs
