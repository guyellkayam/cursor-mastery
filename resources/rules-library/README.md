# Rules Library

> **TL;DR**: Curated .cursorrules and .mdc collections from the community — organized by language, framework, and domain. The awesome-cursorrules repo alone has 35K+ stars.

## Files

| File | What's Inside |
|------|--------------|
| [By Language](by-language.md) | Python, TypeScript, Go, Java, C#, Rust rules |
| [By Framework](by-framework.md) | Next.js, React, FastAPI, Django, Express, Angular |
| [By Domain](by-domain.md) | DevOps, security, data science, blockchain, game dev |
| [Project Templates](project-rules-templates.md) | Starter .cursor/rules/ sets per project type |
| [Security Rules](security-rules.md) | matank001/cursor-security-rules + shift-left patterns |

## Where Rules Come From

| Source | Stars | Format | Focus |
|--------|-------|--------|-------|
| [PatrickJS/awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules) | 35K+ | Mixed (.cursorrules + .mdc) | Everything |
| [sanjeed5/awesome-cursor-rules-mdc](https://github.com/sanjeed5/awesome-cursor-rules-mdc) | Community | .mdc only | Modern format |
| [sparesparrow/cursor-rules](https://github.com/sparesparrow/cursor-rules) | Community | Hierarchical | Structured |
| [matank001/cursor-security-rules](https://github.com/matank001/cursor-security-rules) | Community | Security-focused | Security |
| [blefnk/awesome-cursor-rules](https://github.com/blefnk/awesome-cursor-rules) | Community | Modern stack | Next.js/React/TS |
| [cursor.directory](https://cursor.directory) | 75K devs | .mdc | Community hub |

## How to Install Rules

```bash
# 1. Create rules directory
mkdir -p .cursor/rules

# 2. Browse awesome-cursorrules for your stack
# https://github.com/PatrickJS/awesome-cursorrules

# 3. Copy relevant .mdc files to .cursor/rules/
# Or download from cursor.directory

# 4. Customize globs and descriptions for your project
```

## .mdc Format Quick Reference
```markdown
---
description: When this rule applies
globs: ["*.py"]        # Auto-attach to matching files
alwaysApply: false      # Apply to every request?
---

# Rule Title
Your instructions in Markdown.
```
