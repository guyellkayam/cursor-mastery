# Notification Hooks

> **TL;DR**: Scripts that send notifications to external systems when agent work completes — Jira ticket updates, Gmail summaries, Confluence page updates, and terminal alerts.

## Jira Ticket Update

Automatically updates a Jira ticket when related work is committed.

```bash
#!/bin/bash
# .cursor/hooks/jira-update.sh
# Updates Jira ticket when commits reference a ticket ID

JIRA_BASE_URL="${JIRA_BASE_URL:-https://your-org.atlassian.net}"
JIRA_EMAIL="${JIRA_EMAIL}"
JIRA_API_TOKEN="${JIRA_API_TOKEN}"

# Extract ticket ID from branch name (e.g., feature/PROJ-123-add-auth)
BRANCH=$(git branch --show-current 2>/dev/null)
TICKET_ID=$(echo "$BRANCH" | grep -oE '[A-Z]+-[0-9]+' | head -1)

if [ -z "$TICKET_ID" ]; then
  exit 0
fi

# Get recent commit messages
COMMITS=$(git log --oneline -5 --format="%s" 2>/dev/null)
TIMESTAMP=$(date '+%Y-%m-%d %H:%M')

# Build comment
COMMENT="*Automated Update* ($TIMESTAMP)\\n\\nRecent commits:\\n$(echo "$COMMITS" | sed 's/^/- /')"

# Post comment to Jira
curl -s -X POST \
  "$JIRA_BASE_URL/rest/api/3/issue/$TICKET_ID/comment" \
  -H "Content-Type: application/json" \
  -u "$JIRA_EMAIL:$JIRA_API_TOKEN" \
  -d "{\"body\":{\"type\":\"doc\",\"version\":1,\"content\":[{\"type\":\"paragraph\",\"content\":[{\"type\":\"text\",\"text\":\"$COMMENT\"}]}]}}" \
  > /dev/null 2>&1

echo "Jira $TICKET_ID updated with commit summary"
exit 0
```

### "Use this when..."
- Working on ticketed features/bugs
- Teams that track progress via Jira
- Ensuring ticket history reflects actual development activity

---

## Gmail Session Summary

Sends an email summary when a long session ends.

```bash
#!/bin/bash
# .cursor/hooks/gmail-summary.sh
# Sends session summary via Gmail API (requires OAuth setup)

# Uses the Gmail MCP if available, otherwise falls back to basic mail
RECIPIENT="${NOTIFICATION_EMAIL:-your@email.com}"
PROJECT=$(basename "$(git rev-parse --show-toplevel 2>/dev/null)")
BRANCH=$(git branch --show-current 2>/dev/null)
TIMESTAMP=$(date '+%Y-%m-%d %H:%M')

# Build summary
FILES_CHANGED=$(git diff --name-only 2>/dev/null | wc -l | tr -d ' ')
COMMITS_TODAY=$(git log --oneline --since="8 hours ago" 2>/dev/null | wc -l | tr -d ' ')
DIFF_STATS=$(git diff --stat 2>/dev/null | tail -1)

SUBJECT="Cursor Session: $PROJECT ($BRANCH) - $TIMESTAMP"
BODY="Session Summary for $PROJECT

Branch: $BRANCH
Files changed: $FILES_CHANGED
Commits today: $COMMITS_TODAY
Changes: $DIFF_STATS

Recent commits:
$(git log --oneline -10 --since='8 hours ago' 2>/dev/null)
"

# Send via system mail (configure with mailutils or postfix)
if command -v mail &>/dev/null; then
  echo "$BODY" | mail -s "$SUBJECT" "$RECIPIENT"
  echo "Summary email sent to $RECIPIENT"
fi

exit 0
```

---

## Confluence Page Update

Updates a Confluence page with the latest project status.

```bash
#!/bin/bash
# .cursor/hooks/confluence-update.sh
# Updates Confluence page with project status

CONFLUENCE_BASE="${CONFLUENCE_BASE_URL:-https://your-org.atlassian.net/wiki}"
CONFLUENCE_EMAIL="${CONFLUENCE_EMAIL}"
CONFLUENCE_API_TOKEN="${CONFLUENCE_API_TOKEN}"
PAGE_ID="${CONFLUENCE_STATUS_PAGE_ID}"

if [ -z "$PAGE_ID" ]; then
  exit 0
fi

PROJECT=$(basename "$(git rev-parse --show-toplevel 2>/dev/null)")
BRANCH=$(git branch --show-current 2>/dev/null)
TIMESTAMP=$(date '+%Y-%m-%d %H:%M')

# Build status content
STATUS_CONTENT="<h2>$PROJECT Status - $TIMESTAMP</h2>
<p><strong>Branch:</strong> $BRANCH</p>
<p><strong>Recent commits:</strong></p>
<ul>
$(git log --oneline -5 2>/dev/null | sed 's/^/<li>/' | sed 's/$/<\/li>/')
</ul>
<p><strong>Files changed:</strong> $(git diff --name-only 2>/dev/null | wc -l | tr -d ' ')</p>"

# Get current page version
VERSION=$(curl -s "$CONFLUENCE_BASE/rest/api/content/$PAGE_ID" \
  -u "$CONFLUENCE_EMAIL:$CONFLUENCE_API_TOKEN" | \
  python3 -c "import sys,json;print(json.load(sys.stdin)['version']['number'])" 2>/dev/null)

NEW_VERSION=$((VERSION + 1))

# Update page
curl -s -X PUT \
  "$CONFLUENCE_BASE/rest/api/content/$PAGE_ID" \
  -H "Content-Type: application/json" \
  -u "$CONFLUENCE_EMAIL:$CONFLUENCE_API_TOKEN" \
  -d "{\"version\":{\"number\":$NEW_VERSION},\"title\":\"$PROJECT Status\",\"type\":\"page\",\"body\":{\"storage\":{\"value\":\"$STATUS_CONTENT\",\"representation\":\"storage\"}}}" \
  > /dev/null 2>&1

echo "Confluence page $PAGE_ID updated"
exit 0
```

---

## Terminal Bell / Desktop Notification

Simple notification when agent completes a long-running task.

```bash
#!/bin/bash
# .cursor/hooks/notify-complete.sh
# Sends desktop notification when agent task completes

TITLE="Cursor Agent Complete"
MESSAGE="Task finished in $(basename "$(pwd)")"

# macOS notification
if command -v osascript &>/dev/null; then
  osascript -e "display notification \"$MESSAGE\" with title \"$TITLE\" sound name \"Glass\""
fi

# Linux notification
if command -v notify-send &>/dev/null; then
  notify-send "$TITLE" "$MESSAGE"
fi

# Terminal bell (works everywhere)
printf '\a'

exit 0
```

### "Use this when..."
- Running long agent tasks in the background
- Working on multiple projects simultaneously
- Wanting immediate feedback when agents finish

---

## Subagent Completion Aggregator

Tracks background agent completions and triggers actions when all are done.

```bash
#!/bin/bash
# .cursor/hooks/subagent-complete.sh
# Tracks subagent completions

PROJECT_ROOT=$(git rev-parse --show-toplevel 2>/dev/null || pwd)
TRACKER="$PROJECT_ROOT/.cursor/subagent-tracker.json"
AGENT_ID="${CURSOR_AGENT_ID:-unknown}"
TIMESTAMP=$(date '+%Y-%m-%d %H:%M:%S')

mkdir -p "$PROJECT_ROOT/.cursor"

# Initialize tracker if not exists
if [ ! -f "$TRACKER" ]; then
  echo '{"completed":[],"total":0}' > "$TRACKER"
fi

# Add completion
python3 -c "
import json
with open('$TRACKER', 'r') as f:
    data = json.load(f)
data['completed'].append({'id': '$AGENT_ID', 'time': '$TIMESTAMP'})
with open('$TRACKER', 'w') as f:
    json.dump(data, f, indent=2)
print(f'Subagent {len(data[\"completed\"])} completed')
" 2>/dev/null

exit 0
```

---

## Token Strategy

- Notification hooks run **after** agent work — they don't block or consume tokens
- Use Jira/Confluence hooks to eliminate manual status reporting (saves human time, not tokens)
- Desktop notifications prevent idle waiting — start the agent, work on something else, get pinged when done
- Subagent tracker enables workflow orchestration without using context window for tracking
