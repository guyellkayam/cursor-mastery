# DevOps Workflows for Cursor

> **TL;DR**: Copy-paste Cursor agent workflows for IaC review, CI/CD generation, GitOps manifests, observability setup, and security scanning.

## Workflow 1: Terraform Module Creation

```
Plan Mode (Shift+Tab):
"Create a Terraform module for [resource]:

1. Search existing modules in terraform/modules/ for patterns
2. Create the module structure:
   - main.tf — resources
   - variables.tf — inputs with descriptions, types, validation
   - outputs.tf — useful outputs
   - versions.tf — provider and terraform constraints
   - README.md — usage documentation
3. Follow the tagging convention: environment, team, project
4. Add to the root module with appropriate variables
5. Run terraform fmt and terraform validate

Reference: @file:terraform/modules/ for existing patterns"
```

## Workflow 2: GitHub Actions Workflow

```
"Create a GitHub Actions workflow for [purpose]:

1. Check existing workflows in .github/workflows/ for patterns
2. Create the workflow with:
   - Descriptive name
   - Appropriate trigger (push, PR, schedule)
   - Matrix strategy if multi-version
   - Caching for dependencies
   - Pinned action versions
   - Timeout-minutes
   - Concurrency group
3. Add secrets to GitHub Settings guidance
4. Test by pushing to a feature branch

Reference: @file:.github/workflows/ for patterns"
```

## Workflow 3: ArgoCD Application Setup

```
"Set up ArgoCD Application for [service]:

1. Create ApplicationSet or Application manifest
2. Configure:
   - Source: GitOps repo + path
   - Destination: cluster + namespace
   - Sync policy: automated with self-heal
   - Sync waves if dependencies exist
   - Health checks for custom resources
3. Add to App-of-Apps if using that pattern
4. Add notification config for sync failures

Reference existing ArgoCD manifests: @codebase argocd"
```

## Workflow 4: Kubernetes Deployment

```
"Create Kubernetes deployment for [service]:

1. Create Helm chart or raw manifests:
   - Deployment with resource limits
   - Service (ClusterIP or NodePort)
   - ConfigMap for configuration
   - External Secret for sensitive values
   - HPA for autoscaling
   - PDB for availability
   - NetworkPolicy
2. Add readiness and liveness probes
3. Set security context (non-root, read-only fs)
4. Add to ArgoCD for GitOps deployment
5. Validate with helm lint or kubectl --dry-run"
```

## Workflow 5: Observability Setup

```
"Set up monitoring for [service]:

1. Prometheus:
   - ServiceMonitor for metric scraping
   - PrometheusRule for alerting (RED method)
   - Runbook URL in alert annotations
2. Grafana:
   - Dashboard JSON for service metrics
   - Variables for environment/namespace filtering
   - SLO panel on overview dashboard
3. Loki:
   - Structured JSON logging in the service
   - LogQL queries for common issues
4. Alertmanager:
   - Route for service alerts
   - Notification channel config"
```

## Workflow 6: Security Scanning Pipeline

```
"Add security scanning to CI/CD:

1. Container scanning:
   - Trivy scan in GitHub Actions
   - Fail on critical/high CVEs
   - Allow overrides for false positives

2. Policy enforcement:
   - Kyverno policies for pod security
   - Deny privileged containers
   - Require resource limits
   - Enforce image registry whitelist

3. Secret scanning:
   - Gitleaks pre-commit hook
   - GitHub Advanced Security alerts

4. SAST:
   - Semgrep rules for common vulnerabilities
   - Run in CI on every PR

Reference: @file:resources/security/ for rule templates"
```

## Workflow 7: Vault Secrets Integration

```
"Integrate Vault secrets for [service]:

1. Configure Vault auth method (Kubernetes auth)
2. Create Vault policy with least-privilege access
3. Create Vault role mapped to service account
4. Set up External Secrets Operator or CSI driver
5. Create ExternalSecret resource pointing to Vault path
6. Mount secrets in pod spec
7. Test: verify pod can read secrets
8. Document the secret path and rotation policy"
```

## Workflow 8: Incident Response Automation

```
"Set up automated incident response:

1. Create Prometheus alerting rules for [service]:
   - Error rate > 1% for 5 minutes
   - Latency p99 > 1s for 10 minutes
   - Pod restart count > 3 in 15 minutes

2. Create runbook for each alert:
   Use template from @file:resources/document-creation/runbooks-and-ops.md

3. Configure Alertmanager routes:
   - Critical → PagerDuty
   - Warning → team channel
   - Info → logging only

4. Add auto-remediation where possible:
   - HPA for load-related issues
   - PDB for availability"
```
