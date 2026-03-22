# Finance Rules for Cursor

> **TL;DR**: .mdc rules for financial modeling, data accuracy, and compliance.

### financial-modeling.mdc
```markdown
---
description: Financial modeling conventions
globs: ["*.py", "*.ipynb", "finance/**", "models/**"]
---

# Financial Modeling Rules

- Use Decimal type for currency (not float)
- Cross-validate key figures from multiple sources
- Document all assumptions with source and date
- Sensitivity analysis: +/- 10%, 20%, 30% on key variables
- Scenario modeling: base, bull, bear cases
- Version control models — never overwrite
- Audit trail for all transformations
- Round display values only — keep full precision internally
```

### finops.mdc
```markdown
---
description: FinOps and cloud cost management rules
globs: ["*.tf", "terraform/**", "*.py"]
---

# FinOps Rules

- Tag all cloud resources with: cost-center, environment, team, project
- Use spot/preemptible instances for non-critical workloads
- Right-size instances based on actual usage metrics
- Set up AWS Budgets / GCP Budget alerts
- Review Reserved Instances quarterly
- Infracost in CI for cost-of-change estimation
- S3 lifecycle policies for data tiering
- Delete unused resources (EBS volumes, unattached IPs, stale snapshots)
```
