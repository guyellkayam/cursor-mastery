# DevOps Resources

> **TL;DR**: Cursor rules, plugins, MCP servers, and workflows for DevOps — tailored to k3s, ArgoCD, Vault, Terraform, GitHub Actions, Kyverno, Trivy, Falco, Prometheus/Grafana/Loki.

## Files

| File | What's Inside |
|------|--------------|
| [Rules](rules.md) | DevOps-specific .mdc rules for Terraform, k8s, CI/CD, GitOps |
| [Plugins & MCPs](plugins-and-mcps.md) | GitHub, Kubernetes, Hetzner, AWS plugins and MCP servers |
| [Workflows](workflows.md) | IaC review, CI/CD gen, GitOps manifests, observability |

## Your Stack Reference

Based on [devops-zero-to-hero](https://github.com/guyellkayam/devops-zero-to-hero):

| Layer | Tools |
|-------|-------|
| Cloud | AWS, Hetzner |
| IaC | Terraform (modules, remote state, tfsec) |
| Container | Docker, k3s |
| GitOps | ArgoCD (App-of-Apps pattern) |
| CI/CD | GitHub Actions (14 workflows) |
| Security | Kyverno, Trivy, Falco, Gitleaks, Semgrep |
| Secrets | HashiCorp Vault |
| Networking | Linkerd, Nginx Ingress |
| Observability | Prometheus, Grafana, Loki |
| Helm | Helm charts for all services |
