# Project Management Plugins

> **TL;DR**: Cursor marketplace plugins for Linear, Atlassian (Jira/Confluence), and monday.com — manage tickets, update boards, and track progress from your editor.

## Linear Plugin

**Install**: `/add-plugin linear`

**What it does**:
- View and update Linear issues from Cursor
- Create issues directly from code context
- Link commits to issues automatically
- Track sprint progress

**Use this when...**
- You want to update ticket status while coding
- Need to create sub-tasks from implementation details
- Want to reference issue context in agent prompts

**Agent Prompt Example**:
```
Check Linear for my assigned issues this sprint.
For each issue, check if there are related files in the codebase
and suggest an implementation approach.
```

---

## Atlassian Plugin (Jira + Confluence)

**Install**: `/add-plugin atlassian`

**What it does**:
- View and update Jira issues
- Add comments to tickets with code context
- Search Confluence for documentation
- Create Confluence pages from code
- Transition issues (In Progress, Done, etc.)

**Jira Integration**:
```
"Read Jira ticket PROJ-123 and implement the requirements.
When done, transition the ticket to 'In Review'
and add a comment with the PR link."
```

**Confluence Integration**:
```
"Search Confluence for the authentication architecture document.
Use it as context when implementing the new auth endpoint."
```

**Use this when...**
- Working on ticketed features/bugs
- Need to reference Confluence documentation
- Want automated ticket status updates

---

## monday.com Plugin

**Install**: `/add-plugin monday`

**What it does**:
- View and update monday.com boards
- Create items from code context
- Update status and custom fields

---

## MCP Alternative: Atlassian MCP Server

If the marketplace plugin doesn't meet your needs, use the Atlassian MCP server directly:

```json
// .cursor/mcp.json
{
  "mcpServers": {
    "atlassian": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-atlassian"],
      "env": {
        "JIRA_URL": "https://your-org.atlassian.net",
        "JIRA_EMAIL": "your@email.com",
        "JIRA_API_TOKEN": "your-api-token",
        "CONFLUENCE_URL": "https://your-org.atlassian.net/wiki"
      }
    }
  }
}
```

**Tools available**: Search issues, get issue details, add comments, transition issues, search Confluence, get/create/update pages.

---

## Token Strategy

- Use Linear/Jira plugins to pull issue context into agent — saves you from typing requirements
- Atlassian MCP gives more tools but counts toward the 40-tool limit
- Reference Confluence pages via MCP for architecture decisions — cheaper than explaining from memory
