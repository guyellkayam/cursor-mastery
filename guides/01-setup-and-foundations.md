# Guide 01: Setup & Foundations

> **Level**: 1 (Beginner)
> **Prerequisites**: None
> **Official Docs**: https://cursor.com/docs

## What Is Cursor?

Cursor is an AI-native code editor built as a fork of VS Code. It looks and feels like VS Code but adds deeply integrated AI features: intelligent autocomplete (Tab), inline editing (Cmd+K), a chat sidebar (Cmd+L), and a multi-file agent (Composer). Every VS Code extension, theme, and keybinding works in Cursor.

**Key difference from VS Code + Copilot**: Cursor's AI is aware of your entire codebase (via indexing), supports multiple frontier models (Claude, GPT-4, Gemini), and has an agent mode that can run terminal commands and edit multiple files autonomously.

## Installation

### Download
- **macOS**: Download `.dmg` from https://cursor.sh
- **Windows**: Download `.exe` installer
- **Linux**: Download AppImage or use install script

### First Launch
1. Open Cursor
2. Sign in (required for AI features)
3. Import VS Code settings when prompted (extensions, keybindings, themes)
4. Choose your plan (Free tier: 50 slow premium requests/month, Pro: 500 fast)

### Migrate from VS Code
Cursor auto-detects VS Code installations. On first launch:
- Extensions are imported automatically
- Settings sync carries over
- Keybindings preserved
- Themes preserved

You can run both Cursor and VS Code side by side.

## Core AI Features Overview

| Feature | Shortcut | What It Does |
|---------|----------|-------------|
| **Tab** | `Tab` | Smart autocomplete — multi-line, context-aware |
| **Inline Edit** | `Cmd+K` / `Ctrl+K` | Edit selected code with natural language |
| **Chat** | `Cmd+L` / `Ctrl+L` | Ask questions, get explanations (read-only) |
| **Composer** | `Cmd+I` / `Ctrl+I` | Multi-file creation and editing |
| **Agent** | `Cmd+I` (toggle) | Autonomous mode — terminal, browser, multi-file |

## Essential Settings

### Enable Key Features
Go to **Settings > Cursor Settings**:

1. **Cursor Tab**: Enable (autocomplete predictions)
2. **Terminal Tab**: Enable (AI suggestions in terminal)
3. **Codebase Indexing**: Enable (indexes your project for @codebase)
4. **Privacy Mode**: Enable if working on sensitive code (no data stored)

### Model Configuration
**Settings > Models**:
- Default model for Chat: Claude 3.5 Sonnet (recommended)
- Default model for Composer/Agent: Claude 3.5 Sonnet or GPT-4o
- You can add custom API keys under "API Keys" for BYO models
- Toggle models per-session with `Cmd+/`

### Recommended Settings
```json
{
  "cursor.cpp.enableCursorTab": true,
  "cursor.chat.defaultModel": "claude-3.5-sonnet",
  "cursor.general.enableBeta": true,
  "editor.formatOnSave": true,
  "editor.minimap.enabled": false,
  "terminal.integrated.defaultProfile.osx": "zsh"
}
```

## First Session Workflow

### 1. Open a Project
```bash
cursor /path/to/your/project
# Or from terminal:
cursor .
```

### 2. Let Cursor Index
Wait for the indexing indicator (bottom status bar) to complete. This enables `@codebase` searches.

### 3. Try Each Feature

**Tab Autocomplete:**
```python
# Type a comment, then press Tab
# Calculate the fibonacci sequence up to n
# Tab will generate the full function
```

**Inline Edit (Cmd+K):**
1. Select a block of code
2. Press `Cmd+K`
3. Type: "Add error handling and type annotations"
4. Review the diff, accept or reject

**Chat (Cmd+L):**
1. Press `Cmd+L`
2. Ask: "How does authentication work in this project?"
3. Chat searches your codebase and explains

**Composer (Cmd+I):**
1. Press `Cmd+I`
2. Type: "Create a REST API endpoint for user registration with validation"
3. Composer creates/edits multiple files

### 4. Understand the Three Modes

| Mode | Best For | Edits Files? | Runs Commands? |
|------|----------|-------------|----------------|
| **Chat** | Questions, explanations | No | No |
| **Composer** | Multi-file edits | Yes | No |
| **Agent** | Autonomous tasks | Yes | Yes |

**Rule of thumb**: Start with Chat for understanding, use Composer for changes, Agent for complex multi-step tasks.

## File Structure to Know

```
~/.cursor/                    # Global Cursor config
  mcp.json                    # Global MCP servers
  rules/                      # Global rules (rare)

your-project/
  .cursor/                    # Project-specific config
    rules/                    # Project rules (MDC files)
    mcp.json                  # Project MCP servers
  .cursorrules                # Legacy rules file (still works)
  .cursorignore               # Exclude files from AI context
  .cursorindexingignore       # Exclude from indexing only
```

## Common First-Day Issues

| Issue | Fix |
|-------|-----|
| AI features not working | Sign in to your Cursor account |
| Slow responses | Check your plan limits (Settings > Usage) |
| Extensions missing | Reimport from VS Code (Settings > General) |
| Indexing stuck | Restart Cursor, check .cursorignore |
| Terminal AI not working | Enable "Terminal Tab" in Cursor Settings |

## Sources
- [Cursor Official Docs](https://cursor.com/docs)
- [Codecademy: How to Use Cursor AI](https://www.codecademy.com/article/how-to-use-cursor-ai)
- [Builder.io: Cursor Tips for React/Next.js](https://www.builder.io/blog/cursor-ai-tips-react-nextjs)
