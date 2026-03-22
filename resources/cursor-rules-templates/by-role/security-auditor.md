# Security Auditor Rules

> **TL;DR**: Rules for security-focused code review, threat modeling, compliance, and vulnerability scanning.

## Rules Set (Copy to `.cursor/rules/`)

### security-audit-core.mdc
```markdown
---
description: Core security auditing rules
alwaysApply: true
---

# Security Audit Rules

## Code Review Focus
When reviewing code, check for:
1. **Input validation** — All external input sanitized and validated
2. **Authentication** — Proper auth checks on all protected routes
3. **Authorization** — Role-based access control enforced
4. **Data exposure** — No sensitive data in logs, errors, or responses
5. **Injection** — Parameterized queries, no string concatenation for commands
6. **Cryptography** — Strong algorithms, proper key management
7. **Dependencies** — No known vulnerabilities (CVEs)
8. **Configuration** — No default credentials, debug mode disabled in production

## Threat Modeling
For new features, identify:
- Attack surface (what's exposed?)
- Trust boundaries (where does trusted/untrusted data cross?)
- Data flows (where does sensitive data go?)
- Potential threats (STRIDE: Spoofing, Tampering, Repudiation, Info Disclosure, DoS, Elevation)
```

### owasp-rules.mdc
```markdown
---
description: OWASP Top 10 prevention rules
globs: ["*.ts", "*.tsx", "*.py", "*.js", "*.jsx", "*.go"]
---

# OWASP Top 10 Prevention

1. **Broken Access Control** — Deny by default, enforce at server side
2. **Cryptographic Failures** — Encrypt data in transit (TLS) and at rest
3. **Injection** — Use parameterized queries, ORMs, prepared statements
4. **Insecure Design** — Threat model before implementing
5. **Security Misconfiguration** — No default passwords, disable debug mode
6. **Vulnerable Components** — Keep dependencies updated, scan regularly
7. **Authentication Failures** — MFA, rate limiting, secure password storage
8. **Data Integrity Failures** — Verify software updates, sign artifacts
9. **Logging Failures** — Log security events, monitor for anomalies
10. **SSRF** — Validate and sanitize all URLs, allowlist destinations
```

### compliance-rules.mdc
```markdown
---
description: Compliance and regulatory requirements
alwaysApply: false
---

# Compliance Documentation

## SOC 2
- Access control documentation
- Change management procedures
- Incident response plan
- Data encryption at rest and in transit
- Audit logging for all admin actions

## GDPR
- Data inventory (what personal data, where stored, retention period)
- Consent management
- Right to deletion implementation
- Data Processing Agreements with third parties
- Privacy impact assessments for new features

## CIS Benchmarks
- Follow CIS benchmarks for your cloud provider
- Document exceptions with justification
- Regular compliance scans
```

### container-security.mdc
```markdown
---
description: Container and Kubernetes security scanning rules
globs: ["Dockerfile*", "*.yaml", "*.yml", "k8s/**"]
---

# Container Security

## Image Security
- Scan images with Trivy/Grype in CI pipeline
- No critical/high CVEs in production images
- Use minimal base images (alpine, distroless)
- Pin image digests for production deployments

## Kubernetes Security
- Pod Security Standards: restricted profile
- Network Policies: deny-all default
- Non-root containers (runAsNonRoot: true)
- Read-only root filesystem
- No privileged containers
- Service mesh with mTLS (Linkerd/Istio)

## Runtime Security
- Falco rules for runtime anomaly detection
- Kyverno/OPA policies for admission control
- Audit logging for all API server operations
- Regular security assessments
```
