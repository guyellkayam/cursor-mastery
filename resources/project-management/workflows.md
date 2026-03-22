# Project Management Workflows

> **TL;DR**: Cursor agent workflows for sprint planning, backlog grooming, release notes generation, and standup preparation.

## Workflow 1: Sprint Planning

```
"Help me plan Sprint [N]:

1. Read the backlog from Linear/Jira (top priority items)
2. Check team capacity: [X developers, Y days]
3. Estimate effort for each item based on codebase complexity:
   - Search for files that would need changes
   - Count affected components
   - Identify dependencies
4. Suggest which items fit in the sprint
5. Flag any risks or blockers

Output as a sprint plan table with estimates."
```

## Workflow 2: Backlog Grooming

```
"Review the backlog and help prioritize:

1. Read all open tickets from [Linear/Jira project]
2. For each ticket, assess:
   - Technical complexity (search codebase for related code)
   - Dependencies on other tickets
   - Impact on users
3. Suggest priority ordering
4. Identify tickets that need more detail
5. Flag any stale tickets (no updates > 30 days)"
```

## Workflow 3: Release Notes from Git

```
"Generate release notes for version [X.Y.Z]:

1. Run: git log v[previous]..HEAD --oneline
2. Categorize commits:
   - New Features (feat:)
   - Bug Fixes (fix:)
   - Improvements (refactor:, perf:)
   - Documentation (docs:)
3. Write user-friendly descriptions for each
4. Add breaking changes section if any
5. Format using the template from @file:resources/document-creation/release-and-sprint.md"
```

## Workflow 4: Standup Preparation

```
"Prepare my standup update:

1. Check git log for my commits since yesterday:
   git log --author='[name]' --since='yesterday' --oneline
2. Check current branch and WIP changes: git status
3. Read my assigned tickets from Linear/Jira
4. Format as:
   - Yesterday: [completed items]
   - Today: [planned items]
   - Blockers: [any identified blockers]"
```

## Workflow 5: PR Description Generator

```
"Generate a PR description for the current branch:

1. Run: git log main..HEAD --oneline
2. Run: git diff main --stat
3. Read the related ticket from Linear/Jira (branch name contains ticket ID)
4. Write a PR description with:
   - Summary of changes
   - Related ticket link
   - Testing done
   - Screenshots (if UI changes)
   - Checklist (tests pass, docs updated, no breaking changes)"
```

## Workflow 6: Retrospective Notes

```
"Help prepare sprint retrospective:

1. List all tickets completed this sprint (from Linear/Jira)
2. Check git stats: files changed, lines added/removed, contributors
3. Identify:
   - What went well (completed on time, smooth PRs)
   - What didn't go well (bugs, delays, scope changes)
   - What to improve (based on ticket comments and PR review cycles)
4. Format as a retrospective document"
```
