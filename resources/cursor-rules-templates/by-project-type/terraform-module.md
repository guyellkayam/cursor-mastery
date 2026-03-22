# Terraform Module Rules

> **TL;DR**: Rules for Infrastructure as Code with Terraform — state management, module structure, security scanning, and CI/CD integration.

## Rules Set (Copy to `.cursor/rules/`)

### terraform-overview.mdc
```markdown
---
description: Terraform project conventions and module structure
globs: ["*.tf", "*.tfvars", "terraform/**"]
alwaysApply: false
---

# Terraform Conventions

## Module Structure
```
modules/
  module-name/
    main.tf         — Primary resources
    variables.tf    — Input variables with descriptions and types
    outputs.tf      — Output values
    versions.tf     — Provider and terraform version constraints
    README.md       — Module documentation
    examples/       — Example usage
```

## Naming
- Resources: snake_case (aws_s3_bucket.data_lake)
- Variables: snake_case with descriptive names
- Modules: kebab-case directories
- Workspaces: environment-region (prod-us-east-1)

## Variables
- Always include description, type, and default (if applicable)
- Use validation blocks for constraints
- Sensitive variables marked with sensitive = true
- Use .tfvars files for environment-specific values

## State Management
- Remote state with locking (S3 + DynamoDB or Terraform Cloud)
- State file per environment
- Never commit .tfstate files
- Use terraform_remote_state for cross-module references
```

### terraform-security.mdc
```markdown
---
description: Terraform security scanning and hardening
globs: ["*.tf"]
---

# Terraform Security

## Pre-Commit Checks
- terraform fmt — format before commit
- terraform validate — syntax validation
- tfsec / checkov — security scanning
- tflint — linting for best practices

## Security Rules
- No hardcoded secrets in .tf files
- Use AWS Secrets Manager / Vault for sensitive values
- Enable encryption at rest for all storage resources
- Enable logging for all services
- Use least-privilege IAM policies
- Enable VPC flow logs
- Use private subnets for workloads, public only for load balancers

## Provider Pinning
- Pin provider versions: required_providers with version constraints
- Pin Terraform version: required_version
- Use lock file: .terraform.lock.hcl (committed to git)
```

### terraform-ci.mdc
```markdown
---
description: Terraform CI/CD pipeline conventions
globs: [".github/workflows/*terraform*", "*.tf"]
---

# Terraform CI/CD

## Pipeline Stages
1. terraform fmt -check — formatting
2. terraform init — initialize
3. terraform validate — syntax
4. tfsec / checkov — security scan
5. terraform plan — preview changes
6. Manual approval gate
7. terraform apply -auto-approve (only after approval)

## Plan Artifacts
- Always save plan: terraform plan -out=tfplan
- Apply from plan file: terraform apply tfplan
- Never terraform apply without a plan file

## State Locking
- Use DynamoDB table or Terraform Cloud for state locking
- Never run terraform apply concurrently

## Cost Estimation
- Use infracost in CI to estimate cost impact
- Flag changes that increase monthly cost by > 10%
```
