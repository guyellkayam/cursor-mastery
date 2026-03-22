# Cursor AI Mastery — Project Instructions

## What This Repo Is
A comprehensive knowledge base for mastering Cursor AI editor. Contains 13 guides, 88+ resource files across 18 domains, research reports, and a gap-tracking backlog.

## Daily Update Workflow
On every session that involves updating guides or researching the ecosystem:

1. **Read `research/gaps.md` first** — check open items for things you can resolve
2. **During research**, look for new features, tools, or ecosystem changes that affect the guides
3. **After updates**, update `research/gaps.md`:
   - Move resolved items to the Resolved section with today's date
   - Add newly discovered gaps to the Open section with today's date
   - Update the `Last reviewed` date at the top
4. **Save a dated research report** to `research/YYYY-MM-DD-<topic>.md`

## Branch Strategy
- `main` — stable, production-quality content
- `qa` — daily updates land here first
- All changes go through PR from `qa` → `main` with AI review

## Commit Style
- Use conventional commits: `feat:`, `fix:`, `docs:`, `chore:`
- Keep commits atomic — one logical change per commit

## Key Directories
- `guides/` — 13 numbered mastery guides (01-13)
- `resources/` — 88+ reference files across 18 domains
- `research/` — dated research reports + gaps.md backlog

## IMPORTANT Rules
- NEVER remove existing guide content without replacement
- ALWAYS verify URLs and repo references are real before adding
- ALWAYS update gaps.md when resolving or discovering gaps
- Keep guides focused and practical — copy-paste-ready examples preferred
- Resources use standard format: TL;DR, Quick Install, Curated Items, "Use this when...", Token Strategy, Workflows
