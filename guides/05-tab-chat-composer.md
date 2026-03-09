# Guide 05: Tab, Chat & Composer Deep Dive

> **Level**: 2-4
> **Prerequisites**: Guide 01
> **Official Docs**: https://cursor.com/docs

## The Three Core AI Features

Cursor has three distinct AI interaction modes, each designed for different use cases. Understanding when to use each is critical.

## Tab (Autocomplete)

### What It Does
Context-aware, multi-line code predictions. Goes far beyond simple autocomplete — it predicts entire blocks based on your intent.

### How to Use
1. Start typing or write a comment
2. Ghost text appears (grayed out prediction)
3. Press `Tab` to accept, `Esc` to dismiss
4. Press `Tab` again for multi-line expansion

### Examples

**From a comment:**
```python
# Parse CSV file and return list of dicts with name, email, age
# [Tab generates full function]
```

**From function signature:**
```typescript
async function getUserOrders(userId: string): Promise<Order[]> {
  // [Tab predicts the implementation]
}
```

**From test names:**
```typescript
describe('AuthService', () => {
  it('should return JWT token for valid credentials', () => {
    // [Tab predicts test body]
  });
```

### Tab Settings
- **Settings > Cursor Tab**: Enable/disable
- **Suggestion delay**: Adjust response speed
- **Languages**: Enable per-language

### Pro Tips
- Write descriptive comments first — Tab reads them
- Use function signatures with full type annotations
- Tab learns from your project patterns (via indexing)
- Works in terminal too (enable "Terminal Tab")

## Inline Edit (Cmd+K)

### What It Does
Edit selected code with natural language instructions. The AI sees your selection, your instruction, and surrounding context.

### How to Use
1. Select code (or place cursor on a line)
2. Press `Cmd+K` (Mac) or `Ctrl+K` (Windows/Linux)
3. Type your instruction in the prompt bar
4. Review the diff
5. `Cmd+Enter` to accept, `Esc` to reject

### Best Use Cases
- Quick refactors: "Convert to async/await"
- Add error handling: "Wrap in try-catch with proper error types"
- Type annotations: "Add TypeScript types"
- Style changes: "Convert to Tailwind classes"
- Documentation: "Add JSDoc comments"

### Examples

**Before selection:**
```javascript
function calc(a, b, op) {
  if (op === '+') return a + b;
  if (op === '-') return a - b;
  if (op === '*') return a * b;
  if (op === '/') return a / b;
}
```

**Cmd+K prompt:** "Convert to TypeScript with proper types, use switch statement, handle division by zero"

**After:**
```typescript
function calc(a: number, b: number, op: '+' | '-' | '*' | '/'): number {
  switch (op) {
    case '+': return a + b;
    case '-': return a - b;
    case '*': return a * b;
    case '/':
      if (b === 0) throw new Error('Division by zero');
      return a / b;
  }
}
```

### Terminal Cmd+K
Press `Cmd+K` in the terminal to generate commands:
```
Cmd+K: "Find all TypeScript files modified in the last week"
→ find . -name "*.ts" -mtime -7 -not -path "*/node_modules/*"
```

## Chat (Cmd+L)

### What It Does
Conversational AI sidebar for questions, explanations, and code review. **Read-only** — Chat explains but doesn't directly edit files.

### How to Use
1. Press `Cmd+L` (Mac) or `Ctrl+L` (Windows/Linux)
2. Ask your question
3. Use @-mentions for context
4. Chat responds with explanations, code snippets, suggestions

### Best Use Cases
- Understanding code: "How does @auth-middleware.ts work?"
- Architecture questions: "@codebase What pattern do we use for error handling?"
- Debugging: "Why would this query return null?" (paste error + code)
- Learning: "Explain the difference between `getStaticProps` and `getServerSideProps`"

### Add Code to Chat
1. Select code in editor
2. Press `Cmd+Shift+L` to add selection to Chat
3. Ask your question about the selected code

### Chat ≠ Composer
Chat explains and discusses. It doesn't edit files directly. If you want changes, switch to Composer.

## Composer (Cmd+I)

### What It Does
Multi-file AI editor. Describe what you want in natural language, and Composer creates, modifies, and deletes files to achieve it. This is Cursor's most powerful feature.

### How to Use
1. Press `Cmd+I` (Mac) or `Ctrl+I` (Windows/Linux) for floating Composer
2. Press `Cmd+Shift+I` for full-screen Composer
3. Describe the change
4. Review diffs for each file
5. Accept or reject per-file

### Modes

**Normal Composer**: Makes edits, waits for approval.

**Agent Mode** (toggle with `Cmd+.` or press `Cmd+I` twice):
- Runs terminal commands
- Creates/edits multiple files
- Searches codebase autonomously
- Browses web if needed
- Iterates until the task is complete

### Composer Best Practices

**Be specific about scope:**
```
Create a new API endpoint at /api/products:
- GET: list with pagination (limit, offset)
- POST: create with Zod validation
- File: app/api/products/route.ts
```

**Reference existing patterns:**
```
Add a password reset flow following the same
architecture as the email verification in @auth/.
```

**Use iterative steps:**
```
Step 1: Create the database migration for a products table
(Wait for review)
```
Then after accepting:
```
Step 2: Create the Prisma model and repository
```

### Reviewing Diffs
- Composer shows a diff view for each changed file
- Green = additions, Red = deletions
- Accept per-file or all at once
- **Always review before accepting** — AI can introduce subtle bugs

## When to Use What

| Scenario | Tool | Why |
|----------|------|-----|
| Quick one-liner | Tab | Fastest, inline |
| Edit a function | Cmd+K | Focused, shows diff |
| "How does X work?" | Chat | Read-only, exploratory |
| Create new feature | Composer | Multi-file, autonomous |
| Fix a bug across files | Agent | Needs search + edits |
| Generate terminal commands | Cmd+K (terminal) | Quick command help |

## Keyboard Shortcuts Reference

| Action | Mac | Windows/Linux |
|--------|-----|---------------|
| Tab accept | `Tab` | `Tab` |
| Tab partial accept | `Ctrl+→` | `Ctrl+→` |
| Inline edit | `Cmd+K` | `Ctrl+K` |
| Open Chat | `Cmd+L` | `Ctrl+L` |
| Add to Chat | `Cmd+Shift+L` | `Ctrl+Shift+L` |
| Composer (floating) | `Cmd+I` | `Ctrl+I` |
| Composer (full) | `Cmd+Shift+I` | `Ctrl+Shift+I` |
| Agent toggle | `Cmd+.` | `Ctrl+.` |
| Accept change | `Cmd+Enter` | `Ctrl+Enter` |
| Reject change | `Esc` | `Esc` |
| Toggle model | `Cmd+/` | `Ctrl+/` |
| New chat | `Cmd+N` | `Ctrl+N` |

## Sources
- [Cursor Official Docs](https://cursor.com/docs)
- [The Prompt Warrior: 3 Cursor Modes](https://www.thepromptwarrior.com/p/the-3-cursor-ai-modes)
- [Cursor Blog: Agent Best Practices](https://cursor.com/blog/agent-best-practices)
