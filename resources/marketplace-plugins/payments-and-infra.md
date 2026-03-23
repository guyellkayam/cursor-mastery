# Payments & Infrastructure Plugins

> **TL;DR**: Cursor plugins for payments and cloud infrastructure.

| Plugin | Install | What It Does |
|--------|---------|-------------|
| **Stripe** | `/add-plugin stripe` | Payment integration, webhook handlers, test mode |
| **AWS** | `/add-plugin aws` | Full AWS service integration |
| **Hugging Face** | `/add-plugin hugging-face` | Model management, datasets, training |
| **Phantom Connect** | `/add-plugin phantom-connect` | Web3 wallet — swap, sign, manage addresses; MCP for docs; requires `PHANTOM_APP_ID` |
| **Circle** | `/add-plugin circle` | Stablecoin apps — USDC payments, cross-chain transfers, wallets, smart contracts; MCP for SDK/docs |
| **PagerDuty** | `/add-plugin pagerduty` | Incident management, on-call |
| **LaunchDarkly** | `/add-plugin launchdarkly` | Feature flags, progressive rollout |
| **Plain** | `/add-plugin plain` | Customer support — manage threads, customers, tenants, help center articles |

## "Use this when..."

| Scenario | Plugin |
|----------|--------|
| Building payment flows | Stripe |
| Managing AWS infrastructure | AWS |
| ML model management and training | Hugging Face |
