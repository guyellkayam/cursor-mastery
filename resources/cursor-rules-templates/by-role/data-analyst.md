# Data Analyst Rules

> **TL;DR**: Rules for data analysis — reproducibility, notebook hygiene, data validation, visualization standards.

## Rules Set (Copy to `.cursor/rules/`)

### data-analysis-core.mdc
```markdown
---
description: Core data analysis conventions
globs: ["*.py", "*.ipynb", "notebooks/**", "analysis/**"]
---

# Data Analysis Rules

## Reproducibility
- Pin all dependency versions (requirements.txt or pyproject.toml)
- Set random seeds for any stochastic operations
- Document data sources with download dates
- Use relative paths or environment variables (not hardcoded absolute paths)
- Version control notebooks and scripts

## Notebook Hygiene
- Clear all outputs before committing
- Restart kernel and run all cells before sharing
- Use markdown cells for section headers and explanations
- Keep notebooks focused (one analysis per notebook)
- Extract reusable functions into .py modules

## Data Loading
- Validate data schema on load (column names, types, ranges)
- Check for nulls, duplicates, and outliers
- Document data shape and basic statistics
- Use descriptive variable names (df_sales_2024, not df2)
```

### visualization.mdc
```markdown
---
description: Data visualization standards
globs: ["*.py", "*.ipynb"]
---

# Visualization Standards

## General Rules
- Every chart needs a title and axis labels
- Use consistent color palette across analyses
- Include units in axis labels (Revenue ($M), Duration (ms))
- Add data source and date range in subtitle or footnote
- Use appropriate chart types:
  - Bar: comparing categories
  - Line: trends over time
  - Scatter: relationships between variables
  - Histogram: distributions
  - Heatmap: correlations or density

## Libraries
- Matplotlib for publication-quality static plots
- Plotly for interactive exploration
- Seaborn for statistical visualization
- Altair for declarative visualization

## Formatting
- Figure size appropriate for context (presentation vs report)
- Legend placement that doesn't obscure data
- Grid lines: subtle, only when helpful
- Consistent font sizes across figures
```

### data-validation.mdc
```markdown
---
description: Data quality and validation rules
globs: ["*.py", "*.sql"]
---

# Data Validation

## Input Validation
- Check row count matches expected range
- Verify no unexpected nulls in required columns
- Check value ranges (no negative prices, dates in future)
- Verify referential integrity between related datasets
- Check for duplicate records

## Output Validation
- Aggregation sanity checks (totals match sum of parts)
- Trend continuity (no unexplained jumps)
- Cross-validate with known reference data
- Document any assumptions or data quality issues

## Documentation
- Data dictionary: column name, type, description, source
- Known issues: data gaps, quality problems, caveats
- Transformation logic: clear explanation of derived fields
```
