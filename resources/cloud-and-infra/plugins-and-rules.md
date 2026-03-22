# Cloud & Infrastructure Plugins

> **TL;DR**: Cursor marketplace plugins and rules for major cloud platforms.

## Marketplace Plugins

| Plugin | Install | What It Does |
|--------|---------|-------------|
| AWS | `/add-plugin aws` | Full AWS service integration |
| Cloudflare | `/add-plugin cloudflare` | Workers, D1, R2, Pages management |
| Vercel | `/add-plugin vercel` | Deployment, serverless functions, edge config |
| Netlify | `/add-plugin netlify` | Deployment, functions, forms |
| Supabase | `/add-plugin supabase` | Database, auth, storage, edge functions |

## Hetzner Cloud MCP
104 tools across 13 resource domains:
```json
{
  "mcpServers": {
    "hetzner": {
      "command": "npx",
      "args": ["-y", "@hetzner/mcp-server"],
      "env": { "HETZNER_API_TOKEN": "..." }
    }
  }
}
```
**Warning**: 104 tools exceeds 40-tool limit. Use alone or with minimal other MCPs.

## Cloud Platform Rules

### aws-conventions.mdc
```markdown
---
description: AWS resource conventions
globs: ["*.tf", "terraform/**"]
---

# AWS Conventions
- Use IAM roles (not access keys) for services
- Enable CloudTrail for audit logging
- VPC with public/private subnet separation
- S3 buckets: encryption, versioning, lifecycle policies
- RDS: Multi-AZ for production, automated backups
- Use AWS Organizations for multi-account strategy
```

## "Use this when..."
| Scenario | Tool |
|----------|------|
| Managing AWS resources | AWS plugin or Terraform rules |
| Deploying frontend apps | Vercel or Netlify plugin |
| Full-stack with database | Supabase plugin |
| Edge computing / CDN | Cloudflare plugin |
| Budget VPS/cloud | Hetzner MCP |
