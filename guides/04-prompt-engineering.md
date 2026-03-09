# Guide 04: Prompt Engineering for Cursor

> **Level**: 3-4
> **Prerequisites**: Guides 01-03
> **Sources**: Cursor Blog, Prismic.io, community patterns

## Why Prompting Matters

Cursor's AI follows instructions literally. A vague prompt gives vague results. A specific prompt gives exactly what you want. The difference between "add tests" and a well-structured prompt can save hours of back-and-forth.

## The Five Prompt Patterns

### Pattern 1: Specific Task Prompt
Best for single, focused changes.

```
Add input validation to the /api/register endpoint:
- Email: valid format, max 255 chars
- Password: min 8 chars, 1 uppercase, 1 number, 1 special
- Name: required, 2-100 chars, no HTML
Use Zod for validation. Return 400 with field-level errors.
Follow the pattern in @api/auth/login/route.ts.
```

**Key elements**: What to do, specific constraints, reference pattern, error behavior.

### Pattern 2: Plan First, Then Execute
Best for complex multi-file changes.

```
I need to add a notification system. Before writing code:
1. List all files you'll create or modify
2. Describe the data model
3. Explain the notification delivery flow
4. Wait for my approval before implementing
```

Or use **Plan Mode**: Press `Shift+Tab` in Composer to enable. The agent researches your codebase, creates a plan, and waits for approval.

### Pattern 3: Example-Driven Prompt
Best when you want consistency with existing code.

```
Create a new API endpoint for /api/products following
the exact same pattern as @api/users/route.ts:
- Same error handling structure
- Same auth middleware usage
- Same response format
- Add Zod validation for: name, price, categoryId, description
```

### Pattern 4: Constraint Prompt
Best for refactoring and optimization.

```
Refactor @UserService to:
- Extract database queries into @UserRepository
- Keep the same public API (no breaking changes)
- Add proper TypeScript return types
- Do NOT modify any test files
- Do NOT change the auth logic
```

**Power move**: "Do NOT" constraints prevent the AI from going off-script.

### Pattern 5: Debug/Investigate Prompt
Best for understanding and fixing issues.

```
This test is failing with "TypeError: Cannot read property 'id' of undefined":

@failing-test.ts
@user-service.ts

1. Explain why this error occurs
2. Show the data flow that leads to the undefined value
3. Suggest a fix that doesn't change the test expectations
```

## Prompt Quality Ladder

### Bad (Vague)
```
Add tests
```

### Better (Some Context)
```
Add unit tests for the auth service
```

### Good (Specific)
```
Write unit tests for @auth-service.ts covering:
- Successful login with valid credentials
- Failed login with wrong password (return 401)
- Token refresh with expired token (return 403)
- Rate limiting after 5 failed attempts
Use Vitest, follow patterns in @__tests__/user.test.ts
```

### Best (Specific + Constrained + Referenced)
```
Write unit tests for @auth-service.ts:

## Test Cases
1. login() — valid creds → returns JWT + refresh token
2. login() — wrong password → throws AuthError with code "INVALID_CREDENTIALS"
3. login() — locked account → throws AuthError with code "ACCOUNT_LOCKED"
4. refreshToken() — valid → returns new JWT
5. refreshToken() — expired → throws AuthError with code "TOKEN_EXPIRED"

## Requirements
- Use Vitest with describe/it blocks
- Mock @prisma/client, never hit real DB
- Follow exact patterns from @__tests__/user.test.ts
- One assertion per test
- Test file location: __tests__/auth.test.ts
```

## Agent Mode Prompting

Agent mode (Composer with agent toggle) can run terminal commands and browse. Prompt differently:

### Good Agent Prompt
```
Set up a new Express API:
1. Initialize with bun init
2. Install express, zod, typescript, @types/express
3. Create src/index.ts with a health check endpoint
4. Add tsconfig.json with strict mode
5. Add a dev script to package.json
6. Start the server and verify it responds on :3000
```

### Agent with Plan Mode
Press `Shift+Tab` to enable Plan Mode:
```
I want to migrate our auth from JWT to session-based auth.
Research the current implementation, create a migration plan,
and wait for my approval before making changes.
```

The agent will:
1. Search your codebase for auth-related files
2. Create a detailed plan in `.cursor/plans/`
3. Wait for your approval
4. Execute step by step

## Tips for Better Results

### 1. Reference Existing Code
```
Follow the pattern in @existing-file.ts
```
Concrete examples beat abstract descriptions.

### 2. State What NOT To Do
```
Do NOT modify the database schema.
Do NOT change existing test files.
Do NOT use any deprecated APIs.
```

### 3. Break Large Tasks
Instead of one massive prompt, use iterative steps:
```
Step 1: Create the data model and migrations
[review, approve]
Step 2: Create the repository layer
[review, approve]
Step 3: Create the API routes
[review, approve]
Step 4: Add tests
```

### 4. Use Images
Paste screenshots for:
- UI mockups → implement layouts
- Error messages → debug faster
- Design specs → match colors/spacing
- Browser screenshots → fix visual bugs

### 5. Commit Between Steps
```
After each successful change:
1. Review the diff
2. If good, commit
3. Then prompt for the next step
```

If something goes wrong, you can always rollback.

## Sources
- [Cursor Blog: Agent Best Practices](https://cursor.com/blog/agent-best-practices)
- [Prismic: Cursor AI Guide](https://prismic.io/blog/cursor-ai)
- [Builder.io: Cursor Tips](https://www.builder.io/blog/cursor-ai-tips-react-nextjs)
