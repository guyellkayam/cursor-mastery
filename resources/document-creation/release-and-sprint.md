# Release Notes, Changelogs & Sprint Planning

> **TL;DR**: Templates for shipping software — changelogs from git history, release notes for stakeholders, sprint plans, user stories, and backlog items.

## Changelog (Auto-Generated from Git)

### Cursor Agent Prompt
```
Generate a changelog for the latest release.

Read the git log between the last two tags:
git log v1.2.0..v1.3.0 --oneline

Organize by:
- Added (new features)
- Changed (modifications)
- Fixed (bug fixes)
- Removed (removed features)
- Security (security fixes)

Use Keep a Changelog format. Include PR numbers where available.
```

### Template (Keep a Changelog Format)
```markdown
# Changelog

## [1.3.0] - 2024-03-15

### Added
- User authentication with OAuth2 (#123)
- Export to CSV functionality (#145)

### Changed
- Improved dashboard loading time by 40% (#167)
- Updated dependencies to latest versions (#170)

### Fixed
- Fixed pagination bug on search results (#156)
- Resolved memory leak in background worker (#161)

### Security
- Patched XSS vulnerability in comment rendering (#172)

## [1.2.0] - 2024-02-28
...
```

---

## Release Notes (Stakeholder-Facing)

### Template
```markdown
# Release Notes: v[X.Y.Z]

**Release Date**: [YYYY-MM-DD]
**Release Manager**: [name]

## Highlights
[1-2 sentences: the most important thing in this release]

## New Features
### [Feature Name]
[2-3 sentences explaining the feature and its benefit to users]
![screenshot or diagram if applicable]

## Improvements
- [improvement with user benefit]
- [improvement with user benefit]

## Bug Fixes
- Fixed [issue] that caused [user-visible problem]

## Breaking Changes
- [breaking change with migration guide]

## Known Issues
- [known issue with workaround]

## Upgrade Guide
```bash
# Step 1: [instruction]
# Step 2: [instruction]
```

## Contributors
Thanks to everyone who contributed to this release:
- @[username] — [contribution]
```

---

## Sprint Plan

### Template
```markdown
# Sprint [N] Plan

**Sprint Dates**: [start] → [end]
**Sprint Goal**: [one sentence describing the main objective]
**Team Capacity**: [X story points / Y working days]

## Committed Work

### Epic: [Epic Name]
| Story | Points | Assignee | Priority |
|-------|--------|----------|----------|
| [US-1: As a user, I want to...] | 3 | [name] | Must |
| [US-2: As a admin, I want to...] | 5 | [name] | Must |

### Tech Debt / Maintenance
| Task | Points | Assignee |
|------|--------|----------|
| [task] | 2 | [name] |

### Bugs
| Bug | Points | Assignee |
|-----|--------|----------|
| [bug] | 1 | [name] |

## Total: [X] story points

## Risks
- [risk with mitigation]

## Definition of Done
- [ ] Code reviewed and approved
- [ ] Tests written and passing
- [ ] Documentation updated
- [ ] Deployed to staging
- [ ] Product owner sign-off
```

---

## User Story

### Template
```markdown
## [US-XXX] [Story Title]

**As a** [role/persona]
**I want to** [action/capability]
**So that** [benefit/value]

### Acceptance Criteria
- [ ] Given [context], when [action], then [expected result]
- [ ] Given [context], when [action], then [expected result]

### Technical Notes
- [implementation consideration]
- [dependency or constraint]

### Design
[link to mockup/wireframe if applicable]

### Estimation: [X] story points
```

---

## Backlog Item Template

### Template
```markdown
# [TICKET-ID] [Title]

**Type**: Feature | Bug | Tech Debt | Spike
**Priority**: P0 | P1 | P2 | P3
**Estimated Points**: [X]
**Labels**: [backend, frontend, infra, security]

## Description
[Clear description of what needs to be done]

## Context
[Why this is needed, what triggered it]

## Requirements
- [ ] [requirement 1]
- [ ] [requirement 2]

## Out of Scope
- [explicitly excluded]

## Dependencies
- [blocked by / blocks]

## Definition of Done
- [ ] Implementation complete
- [ ] Tests written and passing
- [ ] Code reviewed
- [ ] Documentation updated
```

---

## "Use this when..."

| Scenario | Document |
|----------|----------|
| Cutting a new release | Changelog + Release Notes |
| Starting a new sprint | Sprint Plan |
| Writing a new feature ticket | User Story |
| Grooming the backlog | Backlog Item Template |
| Communicating changes to stakeholders | Release Notes |
