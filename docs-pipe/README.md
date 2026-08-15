# docs-pipe

Documentation-native RAG (Retrieval-Augmented Generation) for Pipe. Indexes a
Markdown documentation tree (like the `docs/` folder) into heading-aware
chunks, stores them in SQLite, and answers questions with **hybrid keyword +
semantic retrieval** and **source citations**.

Requires: `pipe -get docs-pipe` (pulls `sqlite` automatically via the registry)

## Why docs-pipe?

The base `rag-pipe` module embeds *whole documents* as a single vector and does
brute-force search — imprecise for large docs. `docs-pipe` is built for
real documentation:

- **Heading-aware chunking** — splits at `#`/`##`/`###`, keeps fenced code
  blocks intact, tracks line numbers and the heading path (`Bytecode VM > Components > Compiler`).
- **Hybrid retrieval** — TF-IDF keyword scoring (with stopword removal) fused
  with cosine similarity. Weights adapt automatically: local embeddings
  (DeepSeek, 128-dim) lean on keywords; OpenAI embeddings (1536-dim) lean on
  semantics.
- **Incremental re-indexing** — per-file SHA-256, only changed files are
  re-chunked and re-embedded.
- **Cited answers** — `doc_ask` returns the answer plus sources with
  `path:line — heading`.

## Install

```bash
pipe -get sqlite
pipe -get docs-pipe
```

## Functions

| Function | Description |
|----------|-------------|
| `doc_index(path, opts)` | Index a directory (or single file) of Markdown. `opts`: `{lang, chunk_size, db}`. |
| `doc_index_status(idx)` | `{files, chunks, lang}` |
| `doc_search(idx, query, k)` | Top-k chunks: `[{path, heading, line_start, line_end, text, score, chunk_id}]` |
| `doc_ask(idx, question, k)` | `{answer, sources}` with AI-generated answer citing `[n]` |
| `doc_reindex(idx)` | Incrementally update the index from disk |
| `doc_close(idx)` | Persist the SQLite database and release the handle |

## Embeddings & providers

- **DeepSeek / default** — the `embed` builtin falls back to a local 128-dim
  embedding (no API key needed). Semantic search works offline; `doc_ask`
  still needs `DEEPSEEK_API_KEY` for the final answer.
- **OpenAI** — set `ai_provider "openai"` + `OPENAI_API_KEY` for
  `text-embedding-3-small` (1536-dim) and much better semantic recall.
- **Ollama** — set `ai_provider "ollama"` for local `nomic-embed-text`.

## Usage

```pipe
ai_provider "deepseek"      -- local embeddings, DeepSeek for answers
import "docs-pipe"

-- Index the English docs (persistent)
idx: doc_index "docs/en" {lang: "en", db: "pipe_docs.db"}
print (to_str (doc_index_status idx))     -- {files: 28, chunks: 317, lang: en}

-- Search
results: doc_search idx "How does the bytecode VM work?" 3
for r in results
    print ((get r "path") ++ ":" ++ (to_str (get r "line_start")) ++ " — " ++ (get r "heading"))

-- Ask with citations
res: doc_ask idx "What is MCP?" 3
print (get res "answer")
-- sources: [{path, heading, line_start, line_end, text, score, chunk_id}]

doc_close idx                              -- persist to pipe_docs.db
```

## Options (`doc_index`)

| Option | Default | Description |
|--------|---------|-------------|
| `lang` | auto | `"en"`, `"de"`, or auto-detected from `/en/` or `/de/` in the path |
| `chunk_size` | 1200 | Approx. characters per chunk (heading boundaries respected) |
| `db` | `":memory:"` | SQLite path for persistence |

## Notes

- Runs in **tree-walker (TV) mode**. VM mode has a known compiler limitation
  with large module imports (see the SQLite module chapter).
- Only `.md` files are indexed (recursive directory walk).
