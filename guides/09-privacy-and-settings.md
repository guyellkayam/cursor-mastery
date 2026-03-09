# Guide 09: Privacy, Security & Settings

> **Level**: 2-3
> **Prerequisites**: Guide 01
> **Official Docs**: https://cursor.com/docs

## Privacy Modes

Cursor offers different privacy levels depending on your plan and settings.

### Privacy Mode (Pro Feature)
**Settings > General > Privacy Mode**: When enabled:
- Your code is NOT stored on Cursor servers
- Requests are processed and immediately discarded
- No code used for model training
- Required for enterprise/sensitive codebases

### What Data Cursor Sees
Without Privacy Mode:
- Code snippets sent for completion/editing
- File contents referenced in Chat/Composer
- Terminal output (in Agent mode)

With Privacy Mode:
- Same data sent for processing, but nothing is stored after the request completes.

### Enterprise
- SOC 2 compliance
- Self-hosted options
- SSO/SCIM integration
- Audit logs

## .cursorignore (Critical for Security)

Prevent sensitive files from ever reaching the AI:

```gitignore
# .cursorignore

# Secrets - NEVER send to AI
.env
.env.*
*.key
*.pem
*.p12
*.pfx
credentials.json
service-account.json

# Infrastructure secrets
terraform.tfstate
*.tfvars
ansible/vault.yml

# Sensitive configs
docker-compose.override.yml
**/secrets/

# Large binary files (waste context)
*.zip
*.tar.gz
*.pdf
*.db
*.sqlite

# Build artifacts
/dist/
/build/
/.next/
/node_modules/
```

**Critical**: `.cursorignore` is best-effort. For truly sensitive files, don't store them in the project directory at all.

## Model Configuration

### Available Models
Cursor supports multiple AI providers:

| Model | Best For | Speed |
|-------|----------|-------|
| Claude 3.5 Sonnet | Code generation, complex reasoning | Fast |
| Claude 3 Opus | Deep analysis, architecture | Slower |
| GPT-4o | General purpose, fast | Fast |
| GPT-4 Turbo | Complex tasks | Medium |
| Gemini 1.5 Pro | Large context needs | Medium |

### Setting Defaults
**Settings > Models**:
- Set default model per feature (Chat, Composer, Agent)
- Toggle models during sessions with `Cmd+/`
- Add custom API keys for BYO models

### API Keys (BYO Model)
If you have your own API keys:
1. Settings > Models > API Keys
2. Add your OpenAI/Anthropic/Google key
3. Select the model in dropdown
4. Useful for higher rate limits or specific model access

## Keyboard Shortcuts Reference

### AI Features
| Action | Mac | Windows/Linux |
|--------|-----|---------------|
| Tab (accept) | `Tab` | `Tab` |
| Tab (partial) | `Ctrl+→` | `Ctrl+→` |
| Inline Edit | `Cmd+K` | `Ctrl+K` |
| Chat | `Cmd+L` | `Ctrl+L` |
| Add to Chat | `Cmd+Shift+L` | `Ctrl+Shift+L` |
| Composer | `Cmd+I` | `Ctrl+I` |
| Composer Full | `Cmd+Shift+I` | `Ctrl+Shift+I` |
| Agent Toggle | `Cmd+.` | `Ctrl+.` |
| Plan Mode | `Shift+Tab` (in Composer) | `Shift+Tab` |
| Accept | `Cmd+Enter` | `Ctrl+Enter` |
| Reject | `Esc` | `Esc` |
| Switch Model | `Cmd+/` | `Ctrl+/` |
| New Chat | `Cmd+N` | `Ctrl+N` |

### Navigation (VS Code Base)
| Action | Mac | Windows/Linux |
|--------|-----|---------------|
| Command Palette | `Cmd+Shift+P` | `Ctrl+Shift+P` |
| Quick Open | `Cmd+P` | `Ctrl+P` |
| Toggle Terminal | `` Ctrl+` `` | `` Ctrl+` `` |
| Format Document | `Shift+Opt+F` | `Shift+Alt+F` |
| Multi-Cursor | `Cmd+D` | `Ctrl+D` |
| Shortcuts Editor | `Cmd+K, Cmd+S` | `Ctrl+K, Ctrl+S` |

### Customizing Shortcuts
1. `Cmd+Shift+P` > "Preferences: Open Keyboard Shortcuts"
2. Search for the command
3. Click the pencil icon to rebind
4. Or edit JSON directly: `Cmd+Shift+P` > "Open Keyboard Shortcuts (JSON)"

## Settings Deep Dive

### Cursor-Specific Settings
Access via `Cmd+Shift+P` > "Cursor Settings":

| Setting | What It Does | Recommended |
|---------|-------------|-------------|
| Cursor Tab | AI autocomplete | Enabled |
| Terminal Tab | AI in terminal | Enabled |
| Auto-Run | Agent command execution | Allowlist |
| Privacy Mode | No data storage | Depends on needs |
| Codebase Indexing | @codebase search | Enabled |
| Beta Features | Early access | Enabled |

### VS Code Settings That Matter
```json
{
  "editor.formatOnSave": true,
  "editor.defaultFormatter": "esbenp.prettier-vscode",
  "editor.minimap.enabled": false,
  "editor.wordWrap": "on",
  "files.autoSave": "afterDelay",
  "files.autoSaveDelay": 1000,
  "terminal.integrated.defaultProfile.osx": "zsh",
  "git.autofetch": true,
  "git.confirmSync": false
}
```

## Cursor Plans & Pricing

| Feature | Free | Pro | Business |
|---------|------|-----|----------|
| Slow premium requests | 50/month | Unlimited | Unlimited |
| Fast premium requests | - | 500/month | 500/month |
| Agent mode | Limited | Full | Full |
| Privacy mode | - | Yes | Yes |
| Admin controls | - | - | Yes |
| SSO/SCIM | - | - | Yes |

## Sources
- [Cursor Official Docs](https://cursor.com/docs)
- [Cursor Docs: Ignore File](https://cursor.com/docs/reference/ignore-file)
- [Cursor Docs: Keyboard Shortcuts](https://cursor.com/docs/reference/keyboard-shortcuts)
