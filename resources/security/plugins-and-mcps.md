# Security Plugins & Tools

> **TL;DR**: Security scanning tools and integrations for Cursor — Trivy, Semgrep, Gitleaks, Kyverno, Falco.

## Trivy (Container Scanning)

**Integration**: GitHub Actions or local CLI

```yaml
# .github/workflows/security-scan.yml
- name: Trivy scan
  uses: aquasecurity/trivy-action@master
  with:
    image-ref: '${{ env.IMAGE }}'
    format: 'table'
    exit-code: '1'
    severity: 'CRITICAL,HIGH'
```

**Cursor Workflow**:
```
"Scan container images for vulnerabilities:
1. Run trivy image [image:tag]
2. Parse results: critical, high, medium counts
3. For each critical/high finding, suggest fix:
   - Update base image
   - Patch specific package
   - Use alternative image
4. Generate remediation PR if fixes available"
```

## Semgrep (SAST)

```yaml
# .github/workflows/sast.yml
- name: Semgrep scan
  uses: returntocorp/semgrep-action@v1
  with:
    config: 'p/owasp-top-ten'
```

## Gitleaks (Secret Scanning)

```yaml
# .pre-commit-config.yaml
- repo: https://github.com/gitleaks/gitleaks
  rev: v8.18.0
  hooks:
    - id: gitleaks
```

## Kyverno (Policy Enforcement)

**Cursor Workflow**:
```
"Generate Kyverno policies for:
1. Require resource limits on all containers
2. Deny privileged containers
3. Require non-root user
4. Enforce image registry whitelist
5. Require labels (app, team, environment)

Reference: @file:resources/devops/rules.md for k8s conventions"
```

## Falco (Runtime Security)

**Cursor Workflow**:
```
"Create Falco rules for [service]:
1. Detect unexpected process execution
2. Detect file system access anomalies
3. Detect network connection anomalies
4. Detect privilege escalation attempts
5. Alert to Prometheus/Alertmanager"
```
