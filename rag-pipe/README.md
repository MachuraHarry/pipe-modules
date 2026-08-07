# rag-pipe

RAG (Retrieval-Augmented Generation) for Pipe — index documents, search by meaning, ask questions with AI-generated answers.

Requires: `pipe -get sqlite && pipe -get rag-pipe`

## Functions

- `index_create(h, name)` — Create a search index (SQLite table)
- `index_add(idx, text)` — Embed and store a document
- `index_search(idx, query, k?)` — Find top-k most relevant documents
- `index_ask(idx, question)` — Search context + AI-generated answer

## Usage

```pipe
import "sqlite"
import "rag-pipe"

ai_provider "deepseek"
ai_set_key "deepseek" (env "DEEPSEEK_KEY")

h: db_open ":memory:"
idx: index_create h "knowledge"

-- Index documents
index_add idx "Pipe is an AI-native scripting language."
index_add idx "SQLite provides embedded database storage."
index_add idx "The bytecode VM is 7x faster than tree-walking."

-- Search
results: index_search idx "database storage" 2
-- → [(text: "SQLite provides...", score: 0.85), ...]

-- Ask AI
answer: index_ask idx "How does Pipe store data?"
-- → AI answer using relevant context
```
