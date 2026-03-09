# Guide 10: Anti-Patterns & Common Mistakes

> **Level**: All levels
> **Prerequisites**: Guides 01-05 (minimum)

## Critical Anti-Patterns

### 1. Accepting Changes Without Review
**The mistake**: Hitting "Accept All" on agent-generated code without reading diffs.

**Why it's dangerous**: AI code can look correct while containing subtle bugs — wrong variable references, missing edge cases, security vulnerabilities. The code compiles and even passes basic tests but fails in production.

**The fix**: Review every diff. Read the red (deleted) and green (added) lines. If a change looks unclear, ask Chat to explain it before accepting.

### 2. Context Overload
**The mistake**: @-mentioning 10+ files and hoping the AI figures out what's relevant.

```
# BAD:
@file1 @file2 @file3 @file4 @file5 @file6 @file7 @file8
Fix the authentication bug.
```

**Why it fails**: The AI's attention dilutes across too many files. It may mix up patterns from different files or miss critical details.

**The fix**: Reference 2-3 files maximum. Let the agent search if you need broader context.

```
# GOOD:
The login fails after token refresh. Check @auth-service.ts
and @token-middleware.ts. The error is on line 42.
```

### 3. Vague Prompts
**The mistake**: "Fix this", "Make it better", "Add tests"

**Why it fails**: Ambiguity gives the AI freedom to do whatever it wants. You'll get random improvements you didn't ask for.

**The fix**: Be specific about WHAT, WHERE, and HOW:
```
# BAD:
Add error handling

# GOOD:
Add try-catch to the fetchUser function in @user-service.ts.
Catch ApiError specifically and return { error: code, message }
format. Log the full error with console.error.
```

### 4. Endless Conversations
**The mistake**: Using the same chat for hours across multiple features.

**Why it fails**: Conversation history accumulates noise. The AI starts confusing earlier context with current work. Instructions from message #5 conflict with message #45.

**The fix**: New conversation per task. If you need prior context, use `@Past Chats` selectively.

### 5. Ignoring .cursorignore
**The mistake**: Letting the AI index and access everything — node_modules, .env files, build artifacts.

**Why it fails**:
- Security: AI sees your API keys and secrets
- Performance: Indexing bloat slows everything
- Quality: AI references generated code instead of source code

**The fix**: Create `.cursorignore` on day one. Block secrets, build output, large files.

### 6. Rules Overload
**The mistake**: Creating 20+ rule files with 2000+ lines of instructions.

**Why it fails**: Rules eat context tokens. Too many rules = less room for actual code and conversation. Rules also conflict with each other.

**The fix**: Start with 2-3 rules. Add new ones only after you see the AI making repeated mistakes. Keep total rules under 500 lines.

### 7. YOLO Mode in Unsafe Environments
**The mistake**: Enabling "Run Everything" auto-run with access to production databases, cloud infrastructure, or shared environments.

**Why it fails**: The agent might run destructive commands — `rm -rf`, database drops, unintended deployments. It's optimizing for task completion, not safety.

**The fix**: Use allowlist mode. Only auto-approve safe commands (test, lint, build). Block destructive operations. Never point YOLO at production.

### 8. Not Committing Between Steps
**The mistake**: Letting the agent make 20 changes across 15 files without committing.

**Why it fails**: If something goes wrong at step 15, you lose all 14 good steps. No rollback point.

**The fix**: Commit after each logical unit:
```
Agent creates DB schema → review → commit
Agent creates API routes → review → commit
Agent adds tests → review → commit
```

### 9. Fighting the AI Instead of Guiding It
**The mistake**: Re-prompting the same thing 5 times, getting frustrated when the AI doesn't understand.

**Why it fails**: If the AI doesn't understand after 2 attempts, the prompt is the problem, not the AI.

**The fix**:
- Break the task into smaller steps
- Provide concrete examples
- Reference existing code patterns
- State constraints explicitly ("Do NOT change X")
- Try a different model

### 10. Not Using Plan Mode for Complex Tasks
**The mistake**: Jumping straight into implementation for a feature that touches 10+ files.

**Why it fails**: Without a plan, the agent makes decisions on the fly. Those decisions may conflict with your architecture. You end up refactoring the agent's work.

**The fix**: Use Plan Mode (`Shift+Tab`) for anything beyond simple edits. Review the plan before implementation begins.

## Productivity Anti-Patterns

### Using Chat When You Need Composer
Chat is read-only. If you want changes, switch to Composer. Don't copy-paste code from Chat into files manually.

### Not Using Tab
Tab is the fastest AI feature. Many developers disable it or ignore suggestions. Enable it and let it learn your patterns.

### Manual File Navigation
Instead of browsing the file tree, use:
- `Cmd+P` to quick-open files
- `@codebase` to search semantically
- Let the agent find files for you

### Ignoring Images
Paste screenshots for:
- UI bugs (faster than describing)
- Design mockups (implement visually)
- Error messages (complete context)
- Architecture diagrams (visual spec)

## Security Anti-Patterns

| Anti-Pattern | Risk | Fix |
|-------------|------|-----|
| No .cursorignore | Secrets in AI context | Create on day one |
| YOLO + production DB | Data loss | Allowlist mode only |
| Hardcoded keys in rules | Keys in AI prompts | Use `${env:VAR}` |
| Accepting code blindly | Vulnerabilities | Review all diffs |
| Sensitive data in prompts | Data exposure | Anonymize examples |

## The Golden Rules

1. **Review every diff** before accepting
2. **Commit between steps** for safe rollback
3. **Start fresh conversations** per task
4. **Be specific** in prompts — WHAT, WHERE, HOW
5. **Use Plan Mode** for complex tasks
6. **Protect secrets** with .cursorignore
7. **Start simple** — add rules/complexity only when needed
8. **Let the agent search** — don't over-specify context
9. **Use the right tool** — Tab, Cmd+K, Chat, Composer, Agent each have their place
10. **Treat AI as a collaborator** — guide it, review it, iterate

## Sources
- [Cursor Blog: Agent Best Practices](https://cursor.com/blog/agent-best-practices)
- [Cursor Forum: Best Rules Configuration](https://forum.cursor.com/t/best-cursor-rules-configuration/55979)
- [The Register: YOLO Mode Risks](https://www.theregister.com/2025/07/21/cursor_ai_safeguards_easily_bypassed/)
