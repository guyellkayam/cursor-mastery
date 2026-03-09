# Guide 08: Advanced Workflows

> **Level**: 6-8
> **Prerequisites**: Guides 01-07
> **Sources**: Cursor Blog, community patterns

## Workflow 1: Plan → Implement → Verify

The most reliable pattern for complex features.

### Step 1: Plan Mode
```
[Shift+Tab to enable Plan Mode]

I need to implement a complete user invitation system:
- Admin sends invite via email
- Invite link with unique token
- Registration form pre-filled with email
- Token expiration (48 hours)
- Rate limiting on invite sends

Research the codebase, create a detailed plan with
file paths, and wait for my approval.
```

### Step 2: Review Plan
The agent creates a plan in `.cursor/plans/`. Review it:
- Are the file paths correct?
- Does it match your architecture?
- Remove unnecessary steps
- Approve or revise

### Step 3: Implement in Steps
Accept the plan, then guide step by step:
```
Implement Step 1: Database migration for invites table
```
Review, commit. Then:
```
Implement Step 2: InviteService with send/accept/expire logic
```
Review, commit. Continue.

### Step 4: Verify
```
Run all tests. If any fail, fix them without modifying
the test expectations.
```

## Workflow 2: Test-Driven Development

```
Phase 1: Write failing tests
- Login success case
- Login failure case
- Token refresh
- Session expiry

Phase 2: Run tests, confirm they fail

Phase 3: Implement the code to pass all tests
- Do NOT modify tests
- Run tests after each implementation step

Phase 4: Refactor while keeping tests green
```

## Workflow 3: Multi-Model Comparison

Select multiple models simultaneously to compare approaches:

1. Open Composer
2. Select 2-3 models (Claude, GPT-4, Gemini)
3. Submit the same prompt
4. Compare approaches side by side
5. Pick the best solution (or combine)

**Use case**: Architecture decisions, complex algorithms, unfamiliar domains.

## Workflow 4: Git-Based Development

### Branch Per Feature
```bash
git checkout -b feature/user-invites
# Work with agent
# Commit frequently
# PR when done
```

### Agent + Git Commands
```
Create reusable commands in .cursor/commands/:
```

**`.cursor/commands/pr.md`**:
```markdown
Create a pull request for the current branch:
1. Run `git diff main` to see all changes
2. Summarize the changes
3. Create PR with gh cli: title, description, labels
```

Then use: `/pr` in Composer.

### Parallel Work with Worktrees
Cursor supports git worktrees for parallel agent sessions:

```bash
git worktree add ../feature-auth feature/auth
git worktree add ../feature-payments feature/payments
```

Each worktree can have its own Cursor window with a separate agent.

## Workflow 5: Code Review with Agent

### Self-Review Before PR
```
Review all changes on this branch compared to main:
1. Run git diff main
2. Check for:
   - Security issues (exposed secrets, SQL injection)
   - Missing error handling
   - Inconsistent naming
   - Missing tests
   - Dead code
3. Generate a review summary
```

### Review Incoming PR
```
Review PR #42:
1. Check out the branch
2. Read all changed files
3. Identify potential issues
4. Suggest improvements
5. Check test coverage
```

### Built-in Review
Click **"Review" → "Find Issues"** in Composer for line-by-line AI analysis.

## Workflow 6: Debug Mode

For complex bugs:
```
Debug this issue:

Error: "Cannot read property 'user' of undefined"
Stack trace: [paste]

Steps to reproduce:
1. Login as admin
2. Navigate to /settings
3. Click "Save"

Use Debug Mode:
1. Generate hypotheses for the root cause
2. Add logging to verify
3. Reproduce and analyze
4. Apply targeted fix
5. Add regression test
```

Debug Mode generates multiple hypotheses, instruments code, collects data, and narrows down the root cause.

## Workflow 7: Documentation Generation

```
Generate comprehensive documentation for the API:

For each endpoint in @app/api/:
1. Extract route, method, params, body, response
2. Document auth requirements
3. Add example requests/responses
4. Note error codes

Output as OpenAPI/Swagger spec in docs/api.yaml
```

## Workflow 8: Migration & Refactoring

### Database Migration
```
Migrate user profiles from embedded JSON to a separate table:
1. Create new ProfilesTable migration
2. Write data migration script
3. Update Prisma schema
4. Update UserService to use new table
5. Update all queries
6. Add rollback migration
7. Run tests

Do each step separately, wait for my approval.
```

### Dependency Upgrade
```
Upgrade React from 18 to 19:
1. Check breaking changes in React 19
2. List all affected files in our project
3. Create an upgrade plan
4. Implement changes file by file
5. Run tests after each file
```

## Reusable Commands

Create custom slash commands in `.cursor/commands/`:

**`.cursor/commands/fix-issue.md`**:
```markdown
Fix GitHub issue {{issue_number}}:
1. Fetch issue details from GitHub
2. Research the codebase for relevant files
3. Create a plan
4. Implement the fix
5. Add tests
6. Create a commit with message "fix: #{{issue_number}} - [description]"
```

**`.cursor/commands/review.md`**:
```markdown
Code review checklist:
1. git diff main — show all changes
2. Check for security issues
3. Check for missing error handling
4. Verify test coverage
5. Check naming conventions
6. Generate review summary
```

**`.cursor/commands/update-deps.md`**:
```markdown
Update project dependencies:
1. Run `npm outdated` to see outdated packages
2. Identify major version bumps
3. Update patch/minor versions first
4. Run tests after each update
5. Handle major updates one at a time
```

## Sources
- [Cursor Blog: Agent Best Practices](https://cursor.com/blog/agent-best-practices)
- [Prismic: Cursor AI](https://prismic.io/blog/cursor-ai)
- [Vertu: Cursor Tutorial 2026](https://vertu.com/ai-tools/how-to-master-code-generation-and-debugging-with-cursor-ai-editor-tutorial-in-2026/)
