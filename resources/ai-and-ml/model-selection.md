# Model Selection in Cursor

> **TL;DR**: Which AI model to use for which task in Cursor — Claude, GPT-4, Gemini compared across speed, quality, and cost.

## Model Comparison

| Model | Best For | Speed | Quality | Context |
|-------|----------|-------|---------|---------|
| **Claude Sonnet 4** | Agent mode, complex reasoning, code generation | Fast | Excellent | 200K |
| **Claude Opus 4** | Hardest tasks, architecture, nuanced decisions | Slower | Best | 200K |
| **GPT-4o** | General coding, fast iterations | Fast | Very Good | 128K |
| **GPT-4o mini** | Simple completions, Tab autocomplete | Fastest | Good | 128K |
| **Gemini 2.5 Pro** | Large context, long codebases | Medium | Very Good | 1M |
| **Gemini 2.5 Flash** | Quick tasks, large context at lower cost | Fast | Good | 1M |

## Recommended Configuration

### Per-Feature Model Assignment
| Feature | Recommended Model | Why |
|---------|------------------|-----|
| **Tab** (autocomplete) | GPT-4o mini / Gemini Flash | Speed matters most for inline completions |
| **Inline Edit** (Cmd+K) | Claude Sonnet 4 | Best edit quality for targeted changes |
| **Chat** (Cmd+L) | Claude Sonnet 4 | Strong reasoning for Q&A |
| **Composer** (Cmd+I) | Claude Sonnet 4 | Multi-file quality |
| **Agent** (Cmd+.) | Claude Sonnet 4 | Best tool use and autonomous capability |
| **Plan Mode** | Claude Opus 4 or Sonnet 4 | Architecture decisions need top quality |

### Switching Models
`Cmd+/` to switch models mid-conversation.

## Multi-Model Strategy

### Quick Tasks → Fast Model
```
Tab completion, simple fixes, formatting → GPT-4o mini / Gemini Flash
```

### Standard Development → Balanced Model
```
Feature implementation, bug fixes, tests → Claude Sonnet 4 / GPT-4o
```

### Complex Architecture → Premium Model
```
System design, migration planning, security review → Claude Opus 4
```

### Large Codebase Analysis → Large Context
```
Analyzing entire repo, cross-file dependencies → Gemini 2.5 Pro (1M context)
```

## Token Strategy

- Use cheaper models for Tab and simple edits — high volume, low complexity
- Use premium models for Agent and Plan mode — low volume, high impact
- Switch models mid-conversation when task complexity changes
- Gemini 2.5 Pro for "read the entire codebase" tasks — 1M context fits most repos
