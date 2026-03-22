# Session Hooks

> **TL;DR**: Scripts that run at session boundaries — SessionStart loads context and checks environment; PostStop saves progress, generates summaries, and logs costs.

## SessionStart: Load Project Context

Loads project-specific context when a new Cursor session begins.

```bash
#!/bin/bash
# .cursor/hooks/session-start.sh
# Loads project context at session start

PROJECT_ROOT=$(git rev-parse --show-toplevel 2>/dev/null || pwd)
CONTEXT_FILE="$PROJECT_ROOT/.cursor/context.md"
PROGRESS_FILE="$PROJECT_ROOT/.cursor/progress.md"

echo "=== Session Start ==="

# Show last progress if exists
if [ -f "$PROGRESS_FILE" ]; then
  echo ""
  echo "Last session progress:"
  cat "$PROGRESS_FILE"
fi

# Show git status
echo ""
echo "Git status:"
git status --short 2>/dev/null

# Show recent commits
echo ""
echo "Recent commits:"
git log --oneline -5 2>/dev/null

# Show any TODO items
if [ -f "$PROJECT_ROOT/TODO.md" ]; then
  echo ""
  echo "Pending TODOs:"
  head -20 "$PROJECT_ROOT/TODO.md"
fi

# Check environment
echo ""
echo "Environment check:"
node --version 2>/dev/null && echo "Node: OK" || echo "Node: NOT FOUND"
python3 --version 2>/dev/null && echo "Python: OK" || echo "Python: NOT FOUND"
terraform --version 2>/dev/null | head -1 && echo "Terraform: OK" || echo "Terraform: NOT FOUND"
kubectl version --client --short 2>/dev/null && echo "kubectl: OK" || echo "kubectl: NOT FOUND"

exit 0
```

### "Use this when..."
- Resuming work across multiple sessions
- Working on complex projects where context continuity matters
- Teams where multiple people work on the same project

---

## PostStop: Save Progress Summary

Generates a progress summary when the session ends.

```bash
#!/bin/bash
# .cursor/hooks/post-stop.sh
# Saves session progress for next session

PROJECT_ROOT=$(git rev-parse --show-toplevel 2>/dev/null || pwd)
PROGRESS_FILE="$PROJECT_ROOT/.cursor/progress.md"
TIMESTAMP=$(date '+%Y-%m-%d %H:%M:%S')

mkdir -p "$PROJECT_ROOT/.cursor"

# Build progress summary
cat > "$PROGRESS_FILE" << EOF
# Session Progress
**Last updated**: $TIMESTAMP

## Files Modified
$(git diff --name-only 2>/dev/null | head -20)

## Uncommitted Changes
$(git diff --stat 2>/dev/null | tail -1)

## Recent Commits (this session)
$(git log --oneline --since="4 hours ago" 2>/dev/null | head -10)

## Branch
$(git branch --show-current 2>/dev/null)
EOF

echo "Progress saved to $PROGRESS_FILE"
exit 0
```

---

## PostStop: Cost Tracking

Logs estimated token usage per session for budget monitoring.

```bash
#!/bin/bash
# .cursor/hooks/cost-tracker.sh
# Logs session cost estimate

PROJECT_ROOT=$(git rev-parse --show-toplevel 2>/dev/null || pwd)
COST_LOG="$PROJECT_ROOT/.cursor/logs/cost.csv"
TIMESTAMP=$(date '+%Y-%m-%d %H:%M:%S')
SESSION_ID="${CURSOR_SESSION_ID:-unknown}"

mkdir -p "$PROJECT_ROOT/.cursor/logs"

# Create header if file doesn't exist
if [ ! -f "$COST_LOG" ]; then
  echo "timestamp,session_id,files_modified,commits,branch" > "$COST_LOG"
fi

# Collect metrics
FILES_MODIFIED=$(git diff --name-only 2>/dev/null | wc -l | tr -d ' ')
COMMITS=$(git log --oneline --since="4 hours ago" 2>/dev/null | wc -l | tr -d ' ')
BRANCH=$(git branch --show-current 2>/dev/null)

# Log entry
echo "$TIMESTAMP,$SESSION_ID,$FILES_MODIFIED,$COMMITS,$BRANCH" >> "$COST_LOG"

exit 0
```

### "Use this when..."
- Tracking Cursor usage patterns over time
- Budget monitoring for teams
- Identifying which projects/branches consume most resources

---

## SessionStart: Environment Validation

Ensures all required tools and services are available before starting work.

```bash
#!/bin/bash
# .cursor/hooks/env-check.sh
# Validates required tools and services are available

ERRORS=()

# Required CLI tools
REQUIRED_TOOLS=("git" "node" "npm")
for tool in "${REQUIRED_TOOLS[@]}"; do
  if ! command -v "$tool" &>/dev/null; then
    ERRORS+=("Missing required tool: $tool")
  fi
done

# Check for .env file
if [ -f ".env.example" ] && [ ! -f ".env" ]; then
  ERRORS+=("Missing .env file — copy from .env.example")
fi

# Check Node.js version
if command -v node &>/dev/null; then
  NODE_VERSION=$(node --version | sed 's/v//' | cut -d. -f1)
  if [ "$NODE_VERSION" -lt 18 ]; then
    ERRORS+=("Node.js version too old: $(node --version) (need >= 18)")
  fi
fi

# Check for uncommitted changes
if [ -n "$(git status --porcelain 2>/dev/null)" ]; then
  echo "NOTE: You have uncommitted changes from a previous session"
fi

# Report errors
if [ ${#ERRORS[@]} -gt 0 ]; then
  echo "Environment issues detected:"
  for error in "${ERRORS[@]}"; do
    echo "  - $error"
  done
  echo ""
  echo "Fix these before proceeding."
fi

exit 0
```

---

## PostStop: Lessons Capture

Automatically prompts to capture any lessons learned during the session.

```bash
#!/bin/bash
# .cursor/hooks/lessons-capture.sh
# Checks if any new rules should be created from this session

PROJECT_ROOT=$(git rev-parse --show-toplevel 2>/dev/null || pwd)
RULES_DIR="$PROJECT_ROOT/.cursor/rules"
CHANGES=$(git diff --stat 2>/dev/null | tail -1)

if [ -n "$CHANGES" ]; then
  echo ""
  echo "=== Session Summary ==="
  echo "Changes: $CHANGES"
  echo ""
  echo "Reminder: If you learned any new patterns or conventions during this session,"
  echo "consider creating a .cursor/rules/ MDC rule to capture them."
  echo "Rules directory: $RULES_DIR"
fi

exit 0
```

---

## Token Strategy

- Session hooks run **outside** the agent context — zero token cost
- Use SessionStart to inject context that would otherwise require manual @-mentions (saves input tokens)
- PostStop progress files enable session continuity without relying on memory (which costs tokens to maintain)
- Cost tracking helps identify expensive patterns and optimize token usage over time
