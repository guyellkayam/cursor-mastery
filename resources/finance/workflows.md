# Finance Workflows for Cursor

> **TL;DR**: Cursor agent workflows for equity research, FinOps cost analysis, and budget tracking.

## Workflow 1: Equity Research Agent

Based on a 6-step research agent pattern:

```
"Research [COMPANY] for investment analysis:

Step 1 — Company Overview:
Static overview: sector, market cap, P/E, dividend yield, business description.

Step 2 — Industry Analysis (800-1200 words):
Sector trends, competitive positioning, market dynamics, TAM/SAM/SOM.

Step 3 — Financial History:
5-year financials: revenue growth, margins, FCF, debt/equity, ROE.
Management assessment: tenure, track record, insider ownership.

Step 4A — Bull Case (Peter Lynch Framework):
Growth potential, competitive moats, undervaluation thesis.

Step 4B — Bear Case (Charlie Munger Inversion):
What could go wrong? Risks: competition, regulation, cyclicality, debt.

Step 5 — Valuation:
DCF (WACC vs growth sensitivity table), comparable multiples (EV/EBITDA, P/E).
Fair value range: low, mid, high.

Step 6 — Conclusion:
Buy/Hold/Sell recommendation with confidence level and time horizon.

Use AlphaVantage MCP for real-time data if available."
```

## Workflow 2: FinOps Cost Analysis

```
"Analyze cloud costs and recommend optimizations:

1. Review current infrastructure (Terraform state or AWS console)
2. Identify top cost drivers (compute, storage, data transfer)
3. Check for waste:
   - Unused resources (unattached EBS, stale snapshots)
   - Over-provisioned instances (CPU < 20% utilization)
   - Missing spot instances for non-critical workloads
   - Missing Reserved Instances for stable workloads
4. Run infracost on Terraform for cost estimation
5. Generate cost reduction report:
   - Current monthly cost
   - Optimization opportunities with estimated savings
   - Implementation priority (quick wins first)
   - Risk assessment for each optimization"
```

## Workflow 3: Budget Analysis from CSV

```
"Analyze the budget spreadsheet:
@file:data/budget_2024.csv

1. Load and validate data (check for missing values, formatting)
2. Summarize: total income, total expenses, net savings
3. Category breakdown: what % goes to each category
4. Trend analysis: month-over-month changes
5. Anomaly detection: any unusual spikes or drops
6. Visualization: bar chart (categories), line chart (monthly trend)
7. Recommendations: areas to optimize spending"
```

## Workflow 4: Invoice Processing

```
"Process invoices from @file:invoices/march_2024/:

1. Read all PDF invoices in the directory
2. Extract: vendor, date, amount, currency, category
3. Validate: no duplicates, amounts within expected ranges
4. Generate summary spreadsheet:
   - Invoice number, vendor, amount, date, category, status
   - Total by category and vendor
5. Flag any anomalies (duplicate amounts, unusual vendors)"
```
