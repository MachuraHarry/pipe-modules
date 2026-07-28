# Pipe Modules — Curated AI Pipeline Library

7 reusable Pipe (SPR) modules for the Semantic Pipeline Runtime.

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

## Usage

```pipe
import "https://raw.githubusercontent.com/MachuraHarry/pipe-modules/master/sentiment/module.pipe"

tweets: read_lines "tweets.txt"
stats: sentiment_stats tweets
print stats
```

## Install via CLI

```bash
pipe -get sentiment
```

## Contribute

1. Create a folder with your module name
2. Add `module.pipe` with exported functions
3. Open a PR
