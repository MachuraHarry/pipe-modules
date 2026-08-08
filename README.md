# Pipe Modules — Curated AI Pipeline Library

21 reusable Pipe (SPR) modules for the Semantic Pipeline Runtime.

## Available Modules

| # | Module | Description | Functions |
|---|--------|-------------|-----------|
| 1 | `log-analyzer` | Log classification & summarization | `log_analyze`, `log_summarize` |
| 2 | `sentiment` | Sentiment analysis | `sentiment`, `batch_sentiment`, `sentiment_stats` |
| 3 | `code-review` | AI code review | `review`, `rate` |
| 4 | `translate-batch` | Batch translation | `translate_batch`, `translate` |
| 5 | `incident-report` | Security incident analysis | `incident_analyze`, `incident_severity` |
| 6 | `changelog-gen` | AI changelog generation | `changelog`, `changelog_bilingual` |
| 7 | `email-classifier` | Email classification | `classify_email`, `email_batch`, `email_urgent` |
| 8 | `date-formatter` | Date/time utilities | `format_now`, `relative_time`, `is_weekend` |
| 9 | `parallel-runner` | Parallel AI query execution via `>>` | `ask_many`, `summarize_many`, `translate_many` |
| 10 | `pipe-http` | HTTP client with auth | `hget`, `hpost`, `hput`, `hdelete`, `req`, `auth_bearer`, `auth_basic`, `auth_apikey` |
| 11 | `jpipe` | JSON path query | `jp`, `jpick`, `jkeys`, `jflatten` |
| 12 | `pipe-cli` | CLI framework | `app`, `command`, `flag`, `handler`, `run` |
| 13 | `pipe-date` | DateTime utilities | `parse`, `format`, `add_days`, `diff_days`, `relative` |
| 14 | `pipe-test` | Better test assertions | `expect_eq`, `expect_truthy`, `expect_contains`, `with_hooks` |
| 15 | `pipe-tpl` | Template engine | `render`, `render_file` |
| 16 | `pipe-validate` | Schema validation | `validate`, `is_valid` |
| 17 | `pipe-orm` | ORM for Pipe+SQLite | `table`, `col`, `migrate`, `insert`, `select`, `all`, `first`, `count`, `update`, `delete` |
| 18 | `pipe-web` | Web framework (ASP.NET / Express style) | `app`, `route_get`, `post`, `put`, `delete`, `use`, `listen`, `serve`, `json`, `ok`, `text`, `html`, `redirect`, `not_found` |
| 19 | `sqlite` | Pure-Pipe SQL database engine | `db_open`, `db_close`, `db_exec`, `db_query`, `q`, `exec` |
| 20 | `rag-pipe` | RAG pipeline | `index_create`, `index_add`, `index_add_doc`, `index_list`, `index_get`, `index_delete`, `index_count`, `index_search`, `index_ask` |
| 21 | `telegram-bot` | Telegram Bot API (long polling) | `tg_bot`, `tg_me`, `tg_get_updates`, `tg_send_text`, `tg_send_md`, `tg_send_html`, `tg_reply_text`, `tg_send_buttons`, `tg_edit_text`, `tg_delete_message`, `tg_set_reaction`, `tg_answer_callback_query`, `tg_get_chat` |

## Usage

```pipe
import "pipe-web"

app: app "Demo"
route_get app "/hello/:name" fn hello_req
    ok {message: "Hello"}
serve app "0.0.0.0:8080" 300000
```

## Install via CLI

```bash
pipe -get pipe-web
```

## Contribute

1. Create a folder with your module name
2. Add `module.pipe` with exported functions
3. Open a PR
