# Git & GitHub Workflows

> **TL;DR**: Cursor agent workflows for GitHub — PR creation, code review, release automation, and branch management.

## GitHub Plugin
**Install**: `/add-plugin github`

- Create and manage PRs from Cursor
- View and respond to review comments
- Search issues and discussions
- Link code changes to issues

## GitHub MCP Server
```json
{
  "mcpServers": {
    "github": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-github"],
      "env": { "GITHUB_PERSONAL_ACCESS_TOKEN": "ghp_..." }
    }
  }
}
```

## Workflows

### PR Creation
```
"Create a PR for the current branch:
1. git log main..HEAD --oneline (all commits)
2. git diff main --stat (files changed)
3. Generate PR title and description:
   - Summary of changes
   - Related issue link (from branch name)
   - Testing done
4. Create PR using GitHub plugin/MCP"
```

### Code Review Response
```
"Read the review comments on PR #[number]:
1. List all review comments
2. For each comment:
   - Understand the feedback
   - Make the requested change
   - Reply to the comment with what was changed
3. Push updated code
4. Request re-review"
```

### Release Automation
```
"Create a release for version [X.Y.Z]:
1. Generate changelog from git log since last tag
2. Create and push git tag: git tag v[X.Y.Z]
3. Create GitHub Release with changelog
4. Verify CI/CD pipeline triggers for the tag"
```

### Branch Cleanup
```
"Clean up stale branches:
1. List branches merged into main
2. Identify branches with no activity in 30+ days
3. Show the list for confirmation
4. Delete confirmed branches (local and remote)"
```

### CODEOWNERS Setup
```
"Create .github/CODEOWNERS:
1. Analyze the directory structure
2. Map directories to teams/owners:
   - /terraform/ → @infra-team
   - /src/auth/ → @security-team
   - /src/api/ → @backend-team
   - /src/ui/ → @frontend-team
3. Add global fallback owner
4. Document the review process"
```
