# Productivity MCP Servers

> **TL;DR**: MCP servers for project management and communication — Atlassian, Google Calendar, Gmail, Linear.

## Atlassian MCP (Jira + Confluence)
```json
{ "atlassian": { "command": "npx", "args": ["-y", "@anthropic/mcp-atlassian"], "env": { "JIRA_URL": "https://org.atlassian.net", "JIRA_EMAIL": "you@email.com", "JIRA_API_TOKEN": "...", "CONFLUENCE_URL": "https://org.atlassian.net/wiki" } } }
```
**Tools (50+)**: Search/create/update issues, transition status, search/create/update Confluence pages, add comments.
**Warning**: High tool count — may fill the 40-tool limit alone.

## Google Calendar MCP
```json
{ "gcal": { "command": "npx", "args": ["-y", "@anthropic/mcp-google-calendar"], "env": { "GOOGLE_CLIENT_ID": "...", "GOOGLE_CLIENT_SECRET": "...", "GOOGLE_REFRESH_TOKEN": "..." } } }
```
**Tools**: List events, create events, find free time, respond to invitations.

## Gmail MCP
```json
{ "gmail": { "command": "npx", "args": ["-y", "@anthropic/mcp-gmail"], "env": { "GOOGLE_CLIENT_ID": "...", "GOOGLE_CLIENT_SECRET": "...", "GOOGLE_REFRESH_TOKEN": "..." } } }
```
**Tools**: Search messages, read emails, create drafts, list labels.

## Linear MCP
```json
{ "linear": { "command": "npx", "args": ["-y", "@anthropic/mcp-linear"], "env": { "LINEAR_API_KEY": "lin_..." } } }
```
**Tools**: Search issues, create issues, update status, list projects.
