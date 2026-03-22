# DevOps Engineer Rules

> **TL;DR**: Rules for infrastructure, GitOps, observability, and IaC — tailored to a DevOps engineer's daily workflow.

## Rules Set (Copy to `.cursor/rules/`)

### devops-core.mdc
```markdown
---
description: Core DevOps engineering conventions
alwaysApply: true
---

# DevOps Engineering Rules

## Infrastructure as Code
- All infrastructure changes through code — no manual console changes
- Immutable infrastructure: replace, don't patch
- Version control everything: Terraform, Helm charts, manifests, configs
- Review infrastructure changes like code changes (PR required)

## GitOps Principles
- Git is the single source of truth for infrastructure state
- Changes via pull requests, not direct kubectl/terraform apply
- Automated reconciliation (ArgoCD syncs desired state)
- Rollback = git revert

## Observability First
- Every service must expose metrics, logs, and traces
- Alert on symptoms (error rate, latency), not causes (CPU)
- Every alert must have a runbook
- Dashboard hierarchy: overview → service → detailed

## Security Posture
- Principle of least privilege for all service accounts
- Secrets in Vault or cloud secrets manager — never in code
- Network policies: deny-all default, allow specific
- Container images scanned in CI
```

### devops-terraform.mdc
```markdown
---
description: Terraform conventions for DevOps workflows
globs: ["*.tf", "*.tfvars", "terraform/**", "modules/**"]
---

# Terraform Rules

- Module structure: main.tf, variables.tf, outputs.tf, versions.tf, README.md
- Remote state with locking (S3+DynamoDB or Terraform Cloud)
- Pin provider AND terraform versions
- Variables with description, type, validation
- Use data sources instead of hardcoded IDs
- terraform fmt and terraform validate in pre-commit hooks
- tfsec/checkov scanning in CI
- Plan-then-apply workflow (never apply without plan file)
- Tag all resources with: environment, team, project, managed-by=terraform
```

### devops-kubernetes.mdc
```markdown
---
description: Kubernetes manifest and Helm chart conventions
globs: ["*.yaml", "*.yml", "k8s/**", "manifests/**", "helm/**", "charts/**"]
---

# Kubernetes Rules

## Manifests
- Namespaces for isolation (never use default namespace)
- Resource requests AND limits on all containers
- Readiness AND liveness probes
- Pod Disruption Budgets for availability
- Network Policies for security
- Labels: app, version, environment, team

## Helm Charts
- Chart.yaml with proper versioning (semver)
- values.yaml with sensible defaults
- Templates validated with helm lint
- Use helpers (_helpers.tpl) for repeated patterns
- Document all values in values.yaml with comments

## Security
- Non-root containers (runAsNonRoot: true)
- Read-only root filesystem where possible
- No privileged containers in production
- Service accounts with minimal RBAC
```

### devops-cicd.mdc
```markdown
---
description: CI/CD pipeline conventions
globs: [".github/workflows/**", "Jenkinsfile*", ".gitlab-ci*"]
---

# CI/CD Rules

## Pipeline Structure
1. Lint + Format check
2. Unit tests
3. Build artifacts
4. Security scan (SAST, container scan)
5. Integration tests
6. Deploy to staging
7. Smoke tests
8. Deploy to production (manual approval gate)

## GitHub Actions Specifics
- Pin action versions to SHA or tag (NOT @latest)
- Cache dependencies (actions/cache)
- Matrix strategy for multi-version testing
- Concurrency groups to prevent duplicate runs
- Timeout-minutes to prevent hanging jobs
- Use repository secrets — never hardcode
- Reusable workflows for shared patterns

## Deployment
- Blue/green or canary deployments
- Automated rollback on health check failure
- Deployment notifications to team channel
- Post-deploy smoke tests
```

### devops-observability.mdc
```markdown
---
description: Monitoring, logging, and alerting conventions
globs: ["**/monitoring/**", "**/alerts/**", "prometheus/**", "grafana/**", "**/dashboards/**"]
---

# Observability Rules

## Metrics
- RED method for services: Rate, Errors, Duration
- USE method for resources: Utilization, Saturation, Errors
- Prometheus exposition format
- Meaningful metric names: http_requests_total, http_request_duration_seconds

## Alerting
- Alert on symptoms, not causes
- Every alert has a runbook URL in annotations
- Severity levels: critical (page), warning (ticket), info (log)
- Silence alerts during maintenance windows
- Alert fatigue = too many alerts. Tune thresholds regularly.

## Logging
- Structured JSON format
- Fields: timestamp, level, service, request_id, message
- Centralized log aggregation (Loki, ELK, CloudWatch)
- Log retention policy (30 days hot, 90 days cold, archive)

## Dashboards
- Grafana hierarchy: overview → service → detailed
- Include SLO status on overview dashboard
- Use variables for environment/service filtering
```
