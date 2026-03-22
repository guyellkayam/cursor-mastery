# Post-Tool-Use Hooks

> **TL;DR**: Scripts that run AFTER any agent tool call completes — use for auto-formatting, test triggering, change logging, and lint enforcement.

## Auto-Format After Edits

Automatically runs formatter when any source file is modified.

```bash
#!/bin/bash
# .cursor/hooks/auto-format.sh
# Runs appropriate formatter after file edits

FILE="$CURSOR_TOOL_TARGET"

# Skip if not a file edit
if [ -z "$FILE" ]; then
  exit 0
fi

case "$FILE" in
  *.py)
    if command -v ruff &>/dev/null; then
      ruff format "$FILE" 2>/dev/null
      ruff check --fix "$FILE" 2>/dev/null
    elif command -v black &>/dev/null; then
      black "$FILE" 2>/dev/null
    fi
    ;;
  *.ts|*.tsx|*.js|*.jsx)
    if command -v prettier &>/dev/null; then
      prettier --write "$FILE" 2>/dev/null
    fi
    ;;
  *.go)
    if command -v gofmt &>/dev/null; then
      gofmt -w "$FILE" 2>/dev/null
    fi
    ;;
  *.tf)
    if command -v terraform &>/dev/null; then
      terraform fmt "$FILE" 2>/dev/null
    fi
    ;;
  *.yaml|*.yml)
    if command -v yamlfmt &>/dev/null; then
      yamlfmt "$FILE" 2>/dev/null
    fi
    ;;
esac

exit 0
```

### "Use this when..."
- You want consistent formatting without manual intervention
- Working on projects with strict style requirements
- Collaborating with team members who use different formatters

---

## Test Trigger After Code Changes

Runs relevant tests when source files are modified.

```bash
#!/bin/bash
# .cursor/hooks/test-trigger.sh
# Runs related tests after source file changes

FILE="$CURSOR_TOOL_TARGET"

# Skip if not a source file
if [ -z "$FILE" ]; then
  exit 0
fi

# Skip if the modified file is a test file itself
if echo "$FILE" | grep -qE '(test|spec|_test)\.(py|ts|tsx|js|jsx|go)$'; then
  exit 0
fi

case "$FILE" in
  *.py)
    # Find related test file
    TEST_FILE=$(echo "$FILE" | sed 's/\.py$/_test.py/' | sed 's|src/|tests/|')
    if [ -f "$TEST_FILE" ]; then
      echo "Running related tests: $TEST_FILE"
      python -m pytest "$TEST_FILE" -x --tb=short 2>&1 | tail -5
    fi
    ;;
  *.ts|*.tsx)
    # Find related test file
    TEST_FILE=$(echo "$FILE" | sed 's/\.tsx\?$/.test.ts/')
    if [ -f "$TEST_FILE" ]; then
      echo "Running related tests: $TEST_FILE"
      npx jest "$TEST_FILE" --silent 2>&1 | tail -5
    fi
    ;;
  *.go)
    DIR=$(dirname "$FILE")
    echo "Running Go tests in: $DIR"
    cd "$DIR" && go test ./... -count=1 -short 2>&1 | tail -5
    ;;
esac

exit 0
```

---

## Change Logger

Logs every file modification with timestamp for audit trail.

```bash
#!/bin/bash
# .cursor/hooks/change-logger.sh
# Logs file changes with timestamp

FILE="$CURSOR_TOOL_TARGET"
TOOL="$CURSOR_TOOL_NAME"
TIMESTAMP=$(date '+%Y-%m-%d %H:%M:%S')
LOG_FILE=".cursor/change-log.txt"

# Skip if not a file operation
if [ -z "$FILE" ]; then
  exit 0
fi

# Ensure log directory exists
mkdir -p "$(dirname "$LOG_FILE")"

# Log the change
echo "[$TIMESTAMP] $TOOL: $FILE" >> "$LOG_FILE"

# Keep only last 500 entries
if [ -f "$LOG_FILE" ]; then
  tail -500 "$LOG_FILE" > "$LOG_FILE.tmp" && mv "$LOG_FILE.tmp" "$LOG_FILE"
fi

exit 0
```

### "Use this when..."
- You want a local audit trail of all agent modifications
- Debugging what the agent changed during a session
- Teams that need change accountability

---

## Lint-on-Save

Runs linter after edits and reports issues without blocking.

```bash
#!/bin/bash
# .cursor/hooks/lint-on-save.sh
# Runs linter after file modifications

FILE="$CURSOR_TOOL_TARGET"

if [ -z "$FILE" ]; then
  exit 0
fi

case "$FILE" in
  *.py)
    if command -v ruff &>/dev/null; then
      ISSUES=$(ruff check "$FILE" 2>/dev/null | head -5)
      if [ -n "$ISSUES" ]; then
        echo "Lint issues in $FILE:"
        echo "$ISSUES"
      fi
    fi
    ;;
  *.ts|*.tsx|*.js|*.jsx)
    if command -v eslint &>/dev/null; then
      ISSUES=$(npx eslint "$FILE" --format compact 2>/dev/null | head -5)
      if [ -n "$ISSUES" ]; then
        echo "Lint issues in $FILE:"
        echo "$ISSUES"
      fi
    fi
    ;;
  *.tf)
    if command -v tflint &>/dev/null; then
      ISSUES=$(tflint "$FILE" 2>/dev/null | head -5)
      if [ -n "$ISSUES" ]; then
        echo "TFLint issues in $FILE:"
        echo "$ISSUES"
      fi
    fi
    ;;
esac

# Always exit 0 — don't block on lint issues
exit 0
```

---

## Git Diff Summary

Shows a quick diff summary after file edits for awareness.

```bash
#!/bin/bash
# .cursor/hooks/diff-summary.sh
# Shows git diff stats after file modifications

FILE="$CURSOR_TOOL_TARGET"

if [ -z "$FILE" ] || [ ! -d ".git" ]; then
  exit 0
fi

# Show compact diff stats
STATS=$(git diff --stat "$FILE" 2>/dev/null)
if [ -n "$STATS" ]; then
  echo "Changes: $STATS"
fi

exit 0
```

---

## Token Strategy

- Post-tool-use hooks run **after** the agent acts — they don't slow down or block operations
- Auto-format hooks prevent the agent from wasting tokens on formatting conversations
- Test trigger hooks catch regressions immediately — cheaper than debugging later
- Change logging provides audit trail without consuming context window space
