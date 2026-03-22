# Document Creation

> **TL;DR**: Templates and workflows for creating professional documents with Cursor agent — PRDs, runbooks, API docs, release notes, compliance reports, and more.

## Document Types

| Document | File | Best For |
|----------|------|----------|
| [PRD & Tech Specs](prd-and-specs.md) | Product Requirements, Architecture Decision Records | New features, technical decisions |
| [Runbooks & Ops](runbooks-and-ops.md) | Runbooks, Incident Response, RCA, SLO/SLA | Operations, on-call |
| [API & Diagrams](api-and-diagrams.md) | OpenAPI specs, Mermaid diagrams, architecture docs | API design, system architecture |
| [Release & Sprint](release-and-sprint.md) | Changelogs, release notes, sprint plans, user stories | Shipping software |
| [Security & Compliance](security-and-compliance.md) | SOC2, GDPR, ISO27001, pen test reports | Audits, compliance |

## How to Use with Cursor Agent

### Quick Prompt Pattern
```
Create a [document type] for [subject].

Context:
- [relevant background]
- [constraints or requirements]

Use the template from @file:resources/document-creation/[type].md
Format as Markdown with proper headings.
Include: [specific sections needed]
```

### Agent Mode Workflow
1. Open Cursor Agent (Cmd+.)
2. Reference the template: `@file:resources/document-creation/prd-and-specs.md`
3. Describe what you need
4. Review and iterate

## Token Strategy

- Reference templates via @file — agent reads the template format once
- For long documents, use Plan Mode to outline first, then generate sections
- Break large documents into sections — generate each in a focused prompt
- Use subagents for parallel section generation (one agent per section)
