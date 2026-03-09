# Guide 03: Context Management

> **Level**: 3-4
> **Prerequisites**: Guides 01-02
> **Official Docs**: https://cursor.com/docs/context

## Why Context Is Everything

Cursor's AI is only as good as the context it sees. Give it too little context and it hallucinates. Give it too much and it gets confused (and slow). Context management is THE #1 skill that separates beginners from power users.

**The core constraint**: Every AI model has a limited context window. Everything injected — rules, @-mentions, file contents, conversation history — competes for space.

## The @-Mention System

@-mentions are how you control what the AI sees. They're available in Chat, Composer, and Agent mode.

### File & Code References

| @-Mention | What It Does | Example |
|-----------|-------------|---------|
| `@filename` | Include specific file | `@auth.ts` |
| `@foldername` | Include folder contents | `@components/` |
| `@symbol` | Reference function/class | `@handleLogin` |
| `@definitions` | Include type definitions | Useful for API contracts |

### Search & Discovery

| @-Mention | What It Does | When to Use |
|-----------|-------------|-------------|
| `@codebase` | Semantic search across indexed project | "How does X work?" |
| `@web` | Search the internet | Current docs, recent APIs |
| `@docs` | Search custom doc sources | Framework-specific help |
| `@git` | Git history and changes | "What changed recently?" |
| `@recently-changed` | Files modified recently | Narrow context to active work |

### Special References

| @-Mention | What It Does |
|-----------|-------------|
| `@paste` | Include clipboard content |
| `@Branch` | Current git branch context |
| `@Past Chats` | Reference earlier conversations |
| `@NotepadName` | Inject a Notepad's content |
| `@rule-name` | Inject a specific rule |

## Context Strategies

### Strategy 1: Narrow and Precise (Recommended)
```
Refactor @auth.ts to use @UserService class.
Follow the pattern in @api/users/route.ts.
```

**Why it works**: The AI sees exactly the 3 files it needs. No noise.

### Strategy 2: Let the Agent Search
```
Find where we handle payment webhooks and add
retry logic for failed charges.
```

**Why it works**: Agent mode uses grep and semantic search to find relevant files. Good when you don't know exact paths.

### Strategy 3: Codebase-Wide Understanding
```
@codebase How does our authentication flow work
from login to token refresh?
```

**Why it works**: `@codebase` searches the entire indexed project. Use for understanding, not editing.

### Anti-Pattern: Context Dump
```
# DON'T do this:
@file1 @file2 @file3 @file4 @file5 @file6 @file7 @file8
Fix the bug.
```

**Why it fails**: Too many files compete for context. The AI can't focus.

## .cursorignore

Exclude files from ALL AI features (Tab, Chat, Composer, indexing). Uses `.gitignore` syntax.

```gitignore
# .cursorignore

# Build output
/build/
/dist/
/.next/
/out/

# Dependencies
/node_modules/

# Secrets
.env
.env.*
*.key
*.pem

# Large files
*.csv
*.db
*.sqlite
*.log

# Generated code
/generated/
/prisma/generated/

# Vendor
/vendor/
```

### .cursorindexingignore
More targeted: excludes from indexing only (AI can still read files if explicitly @-mentioned).

```gitignore
# .cursorindexingignore
# Don't index, but allow explicit access

/docs/archive/
/data/fixtures/
*.min.js
*.bundle.js
```

## Conversation Management

### Start Fresh Often
Long conversations accumulate noise. The AI starts forgetting earlier context or getting confused by contradictory messages.

**Rules:**
- New conversation per task/feature
- New conversation when the AI seems confused
- New conversation after completing a logical unit

### Reference Past Work
Instead of continuing a stale conversation:
```
@Past Chats I implemented the auth flow in a previous chat.
Now add rate limiting to the login endpoint.
```

### Branch Context
```
@Branch — I'm on the feature/payment-webhooks branch.
Review what's changed and suggest improvements.
```

## Codebase Indexing

### How It Works
Cursor indexes your project for semantic search. This powers `@codebase` queries.

### Check Indexing Status
Look at the bottom status bar for the indexing indicator. A fully indexed project enables:
- Fast `@codebase` searches
- Better Tab completions (more context)
- Agent mode file discovery

### Optimize Indexing
1. Use `.cursorignore` to exclude noise (build dirs, node_modules)
2. Use `.cursorindexingignore` for large but occasionally needed files
3. Keep project focused — avoid monorepo root indexing

## Context Budget Mental Model

Think of your context as a budget:

```
Total Context Window: ~128K-200K tokens
├── System prompt & rules:    ~5-15K tokens
├── @-mentioned files:         ~10-50K tokens
├── Conversation history:      ~5-20K tokens
├── Codebase search results:   ~5-10K tokens
└── Available for generation:  Remaining tokens
```

**Maximize available generation space** by:
1. Keeping rules concise (< 500 lines total)
2. @-mentioning only essential files
3. Starting fresh conversations
4. Using `.cursorignore` aggressively

## Sources
- [Cursor Docs: Context](https://cursor.com/docs/context)
- [Cursor Docs: Ignore Files](https://cursor.com/docs/reference/ignore-file)
- [Cursor Blog: Agent Best Practices](https://cursor.com/blog/agent-best-practices)
