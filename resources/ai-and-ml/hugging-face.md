# Hugging Face Integration

> **TL;DR**: Hugging Face plugin for model management, training, and dataset operations from Cursor.

## Plugin

**Install**: `/add-plugin hugging-face`

**Capabilities**:
- Browse and search models and datasets
- Download models to local environment
- Upload trained models
- Create model cards
- Manage datasets

## MCP Server Alternative

```json
{
  "mcpServers": {
    "huggingface": {
      "command": "npx",
      "args": ["-y", "@huggingface/mcp-server"],
      "env": {
        "HF_TOKEN": "hf_..."
      }
    }
  }
}
```

## Workflows

### Fine-Tuning Workflow
```
"Set up a fine-tuning pipeline:
1. Load dataset from Hugging Face Hub
2. Preprocess and tokenize
3. Configure training (TRL/SFT/DPO)
4. Train with appropriate hyperparameters
5. Evaluate on held-out test set
6. Push trained model to Hub"
```

### Model Evaluation
```
"Evaluate [model] on [benchmark]:
1. Load model and tokenizer
2. Run inference on test dataset
3. Calculate metrics (accuracy, F1, perplexity)
4. Compare with baseline
5. Generate evaluation report"
```
