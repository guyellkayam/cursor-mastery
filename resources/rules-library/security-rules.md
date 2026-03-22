# Security Rules Collection

> **TL;DR**: Security-focused .mdc rules from matank001/cursor-security-rules and community sources.

## Source: matank001/cursor-security-rules

GitHub: [matank001/cursor-security-rules](https://github.com/matank001/cursor-security-rules)

Focused on security in development workflows and AI agent usage.

### Key Rules Included
- Input validation on all external data
- Authentication and authorization checks
- SQL injection prevention
- XSS prevention
- CSRF protection
- Secure headers
- Dependency vulnerability scanning
- Secret management
- Container security
- AI agent safety (preventing agent from executing dangerous commands)

### Install
```bash
# Clone the repo
git clone https://github.com/matank001/cursor-security-rules.git

# Copy relevant .mdc files to your project
cp cursor-security-rules/rules/*.mdc .cursor/rules/
```

## Additional Security Rules

See also:
- [resources/security/rules.md](../security/rules.md) — comprehensive security .mdc rules
- [resources/cursor-rules-templates/by-role/security-auditor.md](../cursor-rules-templates/by-role/security-auditor.md) — security auditor role template
- [resources/cursor-rules-templates/progressive-complexity/expert-30-rules.md](../cursor-rules-templates/progressive-complexity/expert-30-rules.md) — Rule 30: Security Hardening
