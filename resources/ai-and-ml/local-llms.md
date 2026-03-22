# Local LLMs with Cursor

> **TL;DR**: Running local AI models with Ollama for privacy, offline use, and cost savings — routing strategy between local and cloud.

## Ollama Setup

### Install
```bash
# macOS
brew install ollama

# Start server
ollama serve

# Pull models
ollama pull llama3.1
ollama pull codellama
ollama pull deepseek-coder-v2
```

### Configure in Cursor
Cursor supports OpenAI-compatible API endpoints:

```
Settings → Models → Add Custom Model
- API Base: http://localhost:11434/v1
- Model: llama3.1
- API Key: ollama (any string)
```

## Routing Strategy: Local vs Cloud

| Task | Use Local | Use Cloud |
|------|-----------|-----------|
| Tab completion | Yes (speed, privacy) | If quality insufficient |
| Simple code generation | Yes | No |
| Complex architecture | No | Yes (Claude/GPT-4) |
| Sensitive code (secrets, PII) | Yes (data stays local) | No |
| Large codebase analysis | No (limited context) | Yes (Gemini 1M) |
| Offline development | Yes | Not available |

## Recommended Local Models

| Model | Size | Best For |
|-------|------|----------|
| `deepseek-coder-v2` | 16B | Code generation and completion |
| `codellama:34b` | 34B | Code-specific tasks |
| `llama3.1:8b` | 8B | Fast general tasks |
| `llama3.1:70b` | 70B | High quality (needs 48GB+ RAM) |
| `qwen2.5-coder:32b` | 32B | Strong code model |

## Privacy Considerations

- Local models: data NEVER leaves your machine
- Cursor Privacy Mode + Local model = zero data exposure
- Useful for: healthcare, finance, government, defense projects
- Trade-off: lower quality than Claude/GPT-4 for complex tasks
