# Financial Analyst Rules

> **TL;DR**: Rules for financial modeling, data accuracy, audit trails, and compliance in financial analysis workflows.

## Rules Set (Copy to `.cursor/rules/`)

### financial-core.mdc
```markdown
---
description: Core financial analysis conventions
globs: ["*.py", "*.ipynb", "*.xlsx", "finance/**", "models/**"]
---

# Financial Analysis Rules

## Data Accuracy
- Always cite data sources with retrieval dates
- Cross-validate key figures against multiple sources
- Flag any estimated or interpolated values explicitly
- Use precise decimal handling (Decimal type, not float for currency)
- Round display values only — keep full precision in calculations

## Model Standards
- Document all assumptions clearly
- Sensitivity analysis for key variables (+/- 10%, 20%, 30%)
- Scenario modeling: base case, bull case, bear case
- Version control all models (don't overwrite, create new versions)
- Peer review required for models used in decisions

## Audit Trail
- Log all data transformations with input/output
- Track model version and data version used
- Document methodology changes with justification
- Keep historical model outputs for comparison
```

### financial-modeling.mdc
```markdown
---
description: Financial modeling best practices
globs: ["*.py", "*.ipynb", "models/**"]
---

# Financial Modeling

## DCF (Discounted Cash Flow)
- Project free cash flows for 5-10 years
- Terminal value using perpetuity growth or exit multiple
- WACC calculation with documented inputs (risk-free rate, beta, equity premium)
- Sensitivity table: WACC vs growth rate

## Comparable Analysis
- Minimum 5 comparable companies
- Justify inclusion/exclusion of each comp
- Multiples: EV/Revenue, EV/EBITDA, P/E (use median, not average)
- Adjust for company-specific factors (size, growth, margins)

## Report Output
- Executive summary (1 paragraph)
- Key metrics table
- Valuation range (low, mid, high)
- Risk factors and mitigants
- Data sources appendix
```

### financial-compliance.mdc
```markdown
---
description: Financial data compliance and handling rules
alwaysApply: false
---

# Financial Compliance

## Data Handling
- PII (names, account numbers) never in code or logs
- Use anonymized or aggregated data for development/testing
- Encrypt financial data at rest and in transit
- Access controls: need-to-know basis
- Data retention per regulatory requirements

## Reporting
- Clearly distinguish between audited and unaudited figures
- Currency conversion: document exchange rate and date
- Tax calculations: state applicable tax regime
- Regulatory disclaimer where required
- Time period clearly stated (Q1 2024, FY2024, etc.)

## Version Control
- Tag model versions with date and analyst initials
- Changelog for significant methodology changes
- Archive previous versions (never delete)
- Sign-off required before distribution
```
