# AI & ML MCP Servers

> **TL;DR**: MCP servers for AI services — Hugging Face, Ollama, and model management.

## Hugging Face MCP
```json
{ "huggingface": { "command": "npx", "args": ["-y", "@huggingface/mcp-server"], "env": { "HF_TOKEN": "hf_..." } } }
```
**Tools**: Search models, download, upload, manage datasets, create model cards.

## Ollama MCP (Local LLMs)
```json
{ "ollama": { "command": "npx", "args": ["-y", "@ollama/mcp-server"], "env": { "OLLAMA_HOST": "http://localhost:11434" } } }
```
**Tools**: List models, pull models, generate completions, manage local LLM instances.

## "Use this when..."
| Scenario | MCP |
|----------|-----|
| Model management and training | Hugging Face MCP |
| Local LLM management | Ollama MCP |
| Real-time market data | AlphaVantage MCP (in data-mcps.md) |
