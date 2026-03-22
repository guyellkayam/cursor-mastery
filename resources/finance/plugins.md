# Finance Plugins & MCP Servers

> **TL;DR**: Cursor integrations for financial data — Stripe for payments, AlphaVantage for market data, Databricks/Snowflake for analytics.

## Stripe Plugin
**Install**: `/add-plugin stripe`

- Manage payment integrations from Cursor
- Generate Stripe webhook handlers
- Debug payment flows
- Test with Stripe CLI

## AlphaVantage MCP
```json
{
  "mcpServers": {
    "alphavantage": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-alphavantage"],
      "env": {
        "ALPHAVANTAGE_API_KEY": "your-key"
      }
    }
  }
}
```

**Tools**: Stock quotes, time series, fundamentals, forex, crypto data.

**Use this when...**
- Building equity research tools
- Pulling real-time market data into analysis
- Automating financial data collection

## Databricks Plugin
**Install**: `/add-plugin databricks`

- Query data lakehouse from Cursor
- Generate SQL and PySpark code
- Analyze large financial datasets

## Snowflake Plugin
**Install**: `/add-plugin snowflake`

- Query Snowflake data warehouse
- Generate SQL for financial reports
- Data transformation and modeling
