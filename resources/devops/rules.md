# DevOps Rules for Cursor

> **TL;DR**: .mdc rules for Terraform, Kubernetes, CI/CD, GitOps, and observability — copy to `.cursor/rules/`.

## Terraform Rules

### terraform.mdc
```markdown
---
description: Terraform IaC conventions
globs: ["*.tf", "*.tfvars", "terraform/**", "modules/**"]
---

# Terraform

- Module structure: main.tf, variables.tf, outputs.tf, versions.tf, README.md
- Remote state: S3 + DynamoDB locking
- Pin provider versions and Terraform version
- Variables: description + type + validation
- Data sources instead of hardcoded IDs
- terraform fmt before commit
- Tag all resources: environment, team, project, managed-by=terraform
- No secrets in .tf files — use Vault or AWS Secrets Manager
- Use workspaces for environments (dev, staging, prod)
- Cost estimation: infracost in CI
```

## Kubernetes Rules

### kubernetes.mdc
```markdown
---
description: Kubernetes manifest and Helm conventions
globs: ["*.yaml", "*.yml", "k8s/**", "manifests/**", "helm/**", "charts/**"]
---

# Kubernetes / k3s

- Namespaces for isolation (never default)
- Resource requests AND limits on all containers
- Readiness AND liveness probes
- Pod Disruption Budgets for availability
- Network Policies (Kyverno/Calico)
- Labels: app, version, environment, team
- Non-root containers (runAsNonRoot: true)
- Read-only root filesystem where possible
- Pin image tags — never :latest in production
- Use Helm charts for templating
- Values.yaml with sensible defaults
```

## GitHub Actions Rules

### github-actions.mdc
```markdown
---
description: GitHub Actions CI/CD conventions
globs: [".github/workflows/**", ".github/actions/**"]
---

# GitHub Actions

- Pin action versions to SHA or tag
- Cache dependencies (actions/cache)
- Matrix strategy for multi-version testing
- Concurrency groups to prevent duplicate runs
- Timeout-minutes on all jobs
- Repository secrets — never hardcoded
- Reusable workflows in .github/workflows/
- Separate CI (test) from CD (deploy)
- Fail fast on first error
- Job outputs for cross-job communication
```

## ArgoCD / GitOps Rules

### argocd.mdc
```markdown
---
description: ArgoCD GitOps conventions
globs: ["**/argocd/**", "**/applicationset*", "**/app-of-apps*"]
---

# ArgoCD GitOps

- Git is single source of truth
- App-of-Apps pattern for managing multiple applications
- ApplicationSets for dynamic app generation
- Sync waves for deployment ordering (CRDs → operators → apps)
- Health checks defined for custom resources
- Auto-sync with self-heal enabled
- Rollback via git revert (not manual ArgoCD UI)
- Sealed Secrets or External Secrets Operator for secrets
- Notifications to team channel on sync failure
```

## Observability Rules

### observability.mdc
```markdown
---
description: Monitoring, logging, alerting conventions
globs: ["prometheus/**", "grafana/**", "**/alerts/**", "**/dashboards/**", "**/monitoring/**"]
---

# Observability (Prometheus/Grafana/Loki)

## Metrics (Prometheus)
- RED method for services: Rate, Errors, Duration
- USE method for resources: Utilization, Saturation, Errors
- Meaningful names: http_requests_total, http_request_duration_seconds
- Labels: service, endpoint, method, status_code

## Alerting
- Alert on symptoms (error rate > 1%), not causes (CPU > 80%)
- Every alert has a runbook URL annotation
- Severity: critical (page), warning (ticket), info (log)
- Group related alerts to reduce noise

## Logging (Loki)
- Structured JSON format
- Fields: timestamp, level, service, request_id, message
- LogQL for querying: {app="service"} |= "error"

## Dashboards (Grafana)
- Hierarchy: overview → service → detailed
- Variables for environment/service filtering
- SLO status on overview dashboard
- Include time range and auto-refresh
```

## Vault Rules

### vault.mdc
```markdown
---
description: HashiCorp Vault secrets management
globs: ["**/vault/**", "*vault*"]
---

# HashiCorp Vault

- Secrets via Vault API or CSI driver (not env vars with hardcoded values)
- AppRole or Kubernetes auth for service authentication
- Dynamic secrets for databases (short TTL, auto-rotation)
- Transit engine for encryption-as-a-service
- Audit logging enabled
- Policies follow least-privilege
- Regular secret rotation
- Emergency break-glass procedure documented
```

---

## Quick Install

```bash
mkdir -p .cursor/rules
# Copy relevant .mdc blocks above into individual files
# Recommended for DevOps projects: terraform.mdc + kubernetes.mdc + github-actions.mdc
```
