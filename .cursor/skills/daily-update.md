---
name: daily-update
description: Run a daily ecosystem research and guide update cycle. Reads gaps backlog, researches new features/tools, updates guides, and creates a PR from qa to main.
disable-model-invocation: true
argument-hint: "[optional: focus-area]"
---

# Daily Update Cycle

Run the full daily update workflow for the Cursor AI Mastery knowledge base.

## Step 1: Prepare Branch
```bash
git checkout qa
git pull origin main
```

## Step 2: Read Gap Backlog
Read `research/gaps.md` and note all open items. These are priorities for today's research.

## Step 3: Research
Launch parallel research agents to discover:
1. **New Cursor features** — check cursor.com/blog, cursor.com/docs, changelog
2. **Community tools & rules** — search GitHub for new repos, check awesome-cursorrules updates
3. **Ecosystem changes** — MCP servers, marketplace plugins, cursor.directory new additions
4. **Gap-specific research** — focus on open items from gaps.md: $ARGUMENTS

## Step 4: Update Guides & Resources
Apply findings to the relevant files in `guides/` and `resources/`. Prioritize:
- New official features (highest priority)
- Corrections to outdated information
- New marketplace plugins or MCP servers with significant adoption
- New community rules with high star counts
- Open gaps from gaps.md

## Step 5: Update Gap Backlog
Edit `research/gaps.md`:
- Move resolved items to `## Resolved` with today's date
- Add newly discovered gaps to `## Open` with today's date
- Update `Last reviewed:` date

## Step 6: Save Research Report
Write a dated report to `research/YYYY-MM-DD-ecosystem-update.md` with all findings.

## Step 7: Commit and Push
```bash
git add -A
git commit -m "docs: daily ecosystem update YYYY-MM-DD"
git push -u origin qa
```

## Step 8: Create PR
Create a pull request from `qa` → `main` with:
- **Title**: "docs: daily ecosystem update YYYY-MM-DD"
- **Body**: summary of changes + list of resolved gaps

The AI PR review workflow will automatically validate and merge if approved.
