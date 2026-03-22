# Data Analysis Tools & Workflows

> **TL;DR**: Cursor plugins for data platforms (Databricks, Snowflake, Hex) and agent workflows for CSV/Excel analysis, visualization, and reporting.

## Plugins

### Databricks
**Install**: `/add-plugin databricks`
- Query data lakehouse, generate PySpark code, analyze large datasets

### Snowflake
**Install**: `/add-plugin snowflake`
- Query data warehouse, generate SQL, build reports

### Hex
**Install**: `/add-plugin hex`
- Interactive data analysis, notebook-style workflows

### Amplitude
**Install**: `/add-plugin amplitude`
- Product analytics, user behavior analysis, funnel analysis

## Workflows

### CSV/Excel Analysis
```
"Analyze @file:data/sales_q4.csv:
1. Load and describe the dataset (shape, types, head)
2. Data quality: check nulls, duplicates, outliers
3. Summary statistics by category
4. Top 10 by revenue
5. Monthly trend visualization (line chart)
6. Key insights and recommendations"
```

### SQL Report Generation
```
"Generate a SQL report for [business question]:
1. Identify relevant tables and relationships
2. Write optimized SQL query
3. Add comments explaining each CTE/join
4. Include sample output
5. Suggest a Grafana dashboard for ongoing monitoring"
```

### Dashboard Data Prep
```
"Prepare data for a [topic] dashboard:
1. Identify required metrics and dimensions
2. Write SQL/Python to extract and transform data
3. Output as JSON/CSV suitable for chart library
4. Suggest chart types for each metric
5. Include refresh/scheduling recommendations"
```

## Token Strategy
- Use data plugins to query directly — don't paste large datasets into prompts
- For large CSVs, use pandas/polars in the agent to process locally
- Generate visualizations as code (matplotlib/plotly) — cheaper than describing charts in text
