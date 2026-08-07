# rag-pipe

RAG (Retrieval-Augmented Generation) for Pipe — index documents, search by meaning, ask questions with AI-generated answers.

Requires: `pipe -get sqlite && pipe -get rag-pipe`

## Install

```bash
pipe -get sqlite
pipe -get rag-pipe
```

Then in your script:

```pipe
import "sqlite"
import "rag-pipe"
```

## Functions

### Index Management

| Function | Description |
|----------|-------------|
| `index_create(h, name)` | Create a search index (SQLite table) with metadata columns |
| `index_add(idx, text)` | Embed and store a document (no metadata) |
| `index_add_doc(idx, title, source, tags, text)` | Embed and store a document with metadata + timestamp |
| `index_list(idx)` | List all documents `[{id, title, source, tags, snippet, size, created}]`, newest first |
| `index_get(idx, id)` | Get a single document by id |
| `index_delete(idx, id)` | Delete a document by id |
| `index_count(idx)` | Number of indexed documents |

### Search & Ask

| Function | Description |
|----------|-------------|
| `index_search(idx, query, k?)` | Find top-k most relevant documents (includes `title`, `source`, `score`) |
| `index_ask(idx, question)` | Search context + AI-generated answer |

## Usage

```pipe
import "sqlite"
import "rag-pipe"

ai_provider "deepseek"
ai_set_key "deepseek" (env "DEEPSEEK_KEY")

h: db_open ":memory:"   -- or a file path for persistence
idx: index_create h "knowledge"

-- Index documents with metadata
index_add_doc idx "Pipe language" "example.com" "programming" "Pipe is an AI-native scripting language."
index_add_doc idx "SQLite module" "pipe-modules" "database" "SQLite provides embedded database storage."
index_add_doc idx "RAG" "rag-pipe" "ai" "RAG combines retrieval with generation."

-- List documents
docs: index_list idx
print (to_str (index_count idx))   -- 3

-- Search
results: index_search idx "database storage" 2
-- → [{title: "SQLite module", score: 0.85}, ...]

-- Ask AI
answer: index_ask idx "How does Pipe store data?"
-- → AI answer using relevant context
```

## Persistence

Use a file path with `db_open` to persist the index across restarts. Call `db_close` to flush to disk:

```pipe
h: db_open "knowledge.db"
idx: index_create h "knowledge"
-- ... add documents ...
db_close h   -- writes to knowledge.db
```
