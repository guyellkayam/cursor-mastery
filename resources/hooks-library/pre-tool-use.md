# Pre-Tool-Use Hooks

> **TL;DR**: Scripts that run BEFORE any agent tool call — use for security gates, file protection, secret leak prevention, and enforcing conventions.

## Security Gate: Block Writes to Protected Files

Prevents the agent from modifying critical files (configs, secrets, lock files).

```bash
#!/bin/bash
# .cursor/hooks/protect-files.sh
# Blocks edits to protected files

PROTECTED_FILES=(
  ".env"
  ".env.*"
  "*.pem"
  "*.key"
  "*.cert"
  "terraform.tfstate"
  "terraform.tfstate.backup"
  "*.tfvars"
  "package-lock.json"
  "yarn.lock"
  "pnpm-lock.yaml"
  "Pipfile.lock"
  "poetry.lock"
)

FILE="$CURSOR_TOOL_TARGET"

for pattern in "${PROTECTED_FILES[@]}"; do
  if [[ "$FILE" == $pattern ]]; then
    echo "BLOCKED: Cannot modify protected file: $FILE"
    echo "Reason: This file is in the protected files list."
    echo "To override, remove the pattern from .cursor/hooks/protect-files.sh"
    exit 1
  fi
done

exit 0
```

### "Use this when..."
- Working on projects with sensitive configs (.env, terraform.tfvars)
- Teams where lock files should only be modified by package managers
- Preventing accidental modification of certificates or keys

---

## Secret Leak Prevention

Scans staged content for potential secrets before any file write.

```bash
#!/bin/bash
# .cursor/hooks/secret-scanner.sh
# Scans for potential secrets in file content before writing

FILE="$CURSOR_TOOL_TARGET"
CONTENT="$CURSOR_TOOL_CONTENT"

# Common secret patterns
PATTERNS=(
  'AKIA[0-9A-Z]{16}'           # AWS Access Key
  'sk-[a-zA-Z0-9]{48}'         # OpenAI/Anthropic API Key
  'ghp_[a-zA-Z0-9]{36}'        # GitHub Personal Access Token
  'gho_[a-zA-Z0-9]{36}'        # GitHub OAuth Token
  'xoxb-[0-9]+-[a-zA-Z0-9]+'   # Slack Bot Token
  'SG\.[a-zA-Z0-9_-]+'         # SendGrid API Key
  'sk_live_[a-zA-Z0-9]+'       # Stripe Live Key
  'rk_live_[a-zA-Z0-9]+'       # Stripe Restricted Key
  'vault_token=[a-zA-Z0-9]+'   # Vault Token
  'password\s*=\s*["\x27][^"\x27]+'  # Hardcoded passwords
)

for pattern in "${PATTERNS[@]}"; do
  if echo "$CONTENT" | grep -qE "$pattern"; then
    echo "BLOCKED: Potential secret detected in $FILE"
    echo "Pattern matched: $pattern"
    echo "Review the content and use environment variables instead."
    exit 1
  fi
done

exit 0
```

---

## Conventional Commits Enforcer

Ensures commit messages follow the Conventional Commits format.

```bash
#!/bin/bash
# .cursor/hooks/conventional-commits.sh
# Validates commit messages follow conventional commits format

COMMIT_MSG="$CURSOR_TOOL_ARGS"

# Only run for git commit commands
if [[ "$CURSOR_TOOL_NAME" != *"commit"* ]]; then
  exit 0
fi

# Conventional commit pattern: type(scope): description
PATTERN='^(feat|fix|docs|style|refactor|perf|test|build|ci|chore|revert)(\([a-zA-Z0-9_-]+\))?: .{1,72}$'

# Extract first line of commit message
FIRST_LINE=$(echo "$COMMIT_MSG" | head -1)

if ! echo "$FIRST_LINE" | grep -qE "$PATTERN"; then
  echo "BLOCKED: Commit message doesn't follow Conventional Commits format"
  echo ""
  echo "Expected: type(scope): description"
  echo "Got: $FIRST_LINE"
  echo ""
  echo "Valid types: feat, fix, docs, style, refactor, perf, test, build, ci, chore, revert"
  echo "Example: feat(auth): add OAuth2 login flow"
  exit 1
fi

exit 0
```

---

## File Size Guard

Prevents creating excessively large files that could slow down the editor.

```bash
#!/bin/bash
# .cursor/hooks/file-size-guard.sh
# Blocks creation of files larger than a threshold

MAX_LINES=1000
FILE="$CURSOR_TOOL_TARGET"
CONTENT="$CURSOR_TOOL_CONTENT"

LINE_COUNT=$(echo "$CONTENT" | wc -l)

if [ "$LINE_COUNT" -gt "$MAX_LINES" ]; then
  echo "WARNING: File $FILE would have $LINE_COUNT lines (max: $MAX_LINES)"
  echo "Consider splitting into smaller files for better maintainability."
  echo "This is a warning — the write will proceed."
fi

exit 0
```

---

## Terraform Plan Before Apply

Ensures `terraform plan` is run before any `terraform apply` command.

```bash
#!/bin/bash
# .cursor/hooks/terraform-plan-first.sh
# Blocks terraform apply without a preceding plan

COMMAND="$CURSOR_TOOL_COMMAND"

if echo "$COMMAND" | grep -q "terraform apply" && ! echo "$COMMAND" | grep -q "\-plan="; then
  echo "BLOCKED: terraform apply requires a plan file"
  echo ""
  echo "Run 'terraform plan -out=tfplan' first, then 'terraform apply tfplan'"
  echo "This prevents unreviewed infrastructure changes."
  exit 1
fi

exit 0
```

---

## Token Strategy

- Pre-tool-use hooks have **zero token cost** — they run as shell processes outside the context window
- Use hooks for repetitive checks instead of adding them to .cursor/rules/ (rules consume tokens, hooks don't)
- Combine multiple checks in one script to minimize process overhead
- Use exit codes: `0` = allow, `1` = block with message
