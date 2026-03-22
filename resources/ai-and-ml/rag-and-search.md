# RAG & Codebase Search

> **TL;DR**: How Cursor's @codebase indexing works, embeddings-based search, and local RAG patterns for private knowledge bases.

## Cursor's Built-in RAG (@codebase)

### How It Works
1. Cursor indexes your project files into vector embeddings
2. When you use `@codebase`, it performs semantic search
3. Relevant code chunks are injected into the context
4. Agent sees only the most relevant parts, not the entire repo

### Best Practices
```
# Good: Let @codebase find relevant code
"@codebase How does the authentication flow work?"

# Better: Combine @codebase with specific hints
"@codebase Find all database migration files and explain the schema evolution"

# Best: Use Ask mode first, then Agent mode
/ask "@codebase What patterns exist for error handling?"
# Then switch to Agent mode with that knowledge
```

### Indexing Configuration
- Cursor auto-indexes on project open
- `.cursorignore` excludes files from indexing
- Large files (>1MB) may not be fully indexed
- Binary files are excluded

## Local RAG with ChromaDB + Ollama

For private knowledge bases that shouldn't go to cloud:

```python
# Setup: pip install chromadb ollama sentence-transformers
import chromadb
from sentence_transformers import SentenceTransformer

# Create vector store
client = chromadb.PersistentClient(path="./chroma_db")
collection = client.get_or_create_collection("docs")

# Add documents
model = SentenceTransformer('all-MiniLM-L6-v2')
embeddings = model.encode(documents)
collection.add(
    documents=documents,
    embeddings=embeddings.tolist(),
    ids=[f"doc_{i}" for i in range(len(documents))]
)

# Query
results = collection.query(
    query_embeddings=model.encode(["How does auth work?"]).tolist(),
    n_results=5
)
```

### Use Cases for Local RAG
- Internal documentation search
- Private codebase Q&A
- Compliance docs (can't send to cloud)
- Legacy system documentation
- Knowledge base for team onboarding

## Token Strategy

- `@codebase` is token-efficient — only injects relevant chunks, not entire files
- Use `.cursorignore` to exclude noise (node_modules, build, logs)
- Local RAG: pre-process and chunk documents to fit context windows
- Combine local RAG results with Cursor agent for hybrid search
