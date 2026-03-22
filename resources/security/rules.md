# Security Rules for Cursor

> **TL;DR**: .mdc rules for security-focused development — OWASP prevention, container security, secrets management.

### security-core.mdc
```markdown
---
description: Core security rules for all code
alwaysApply: true
---

# Security Core

- Never hardcode secrets, API keys, passwords, or tokens
- Validate and sanitize ALL external input
- Use parameterized queries for database operations
- No dynamic code execution with user-controlled input
- Encrypt sensitive data at rest and in transit
- Use HTTPS/TLS for all external communications
- Implement rate limiting on public endpoints
- Log security events (auth failures, permission denials)
- Keep dependencies updated — check for CVEs regularly
```

### container-security.mdc
```markdown
---
description: Container and image security rules
globs: ["Dockerfile*", "*.yaml", "*.yml", "k8s/**"]
---

# Container Security

## Images
- Scan with Trivy in CI (fail on critical/high)
- Use minimal base images (alpine, distroless)
- Pin image digests in production
- No root user (USER nonroot in Dockerfile)
- Multi-stage builds (separate build from runtime)

## Kubernetes
- Pod Security Standards: restricted profile
- Network Policies: deny-all default, allow specific
- Non-root, read-only filesystem
- No privileged containers
- Service accounts with minimal RBAC
- Kyverno policies for admission control
```

### secrets-management.mdc
```markdown
---
description: Secrets management rules
globs: ["*.tf", "*.yaml", "*.yml", "*.py", "*.ts", "*.js", "*.env*"]
---

# Secrets Management

- Secrets in Vault, AWS Secrets Manager, or sealed secrets
- Never in environment variables directly (use secret references)
- .gitignore: .env, .env.*, *.pem, *.key, *.cert, *.tfvars
- .cursorignore: same patterns (AI shouldn't see secrets)
- Rotate secrets on schedule and after incidents
- Audit secret access logs
- Different secrets per environment (dev/staging/prod)
```

### shift-left-security.mdc
```markdown
---
description: Shift-left security practices
alwaysApply: false
---

# Shift-Left Security

## In Development (Agent rules)
- Check for OWASP Top 10 in every code change
- Validate auth/authz on all new endpoints
- Input validation before any processing
- SQL injection prevention (parameterized queries)

## In CI Pipeline
- Gitleaks for secret scanning
- Semgrep for SAST (static analysis)
- Trivy for container scanning
- tfsec/checkov for IaC scanning
- Dependency scanning (npm audit, pip-audit)

## In Runtime
- Falco for anomaly detection
- Kyverno for policy enforcement
- Audit logging for all admin actions
- Rate limiting and DDoS protection
```
