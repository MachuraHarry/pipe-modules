# sentiment

AI-powered sentiment analysis for texts.

## Functions

- `analyze_sentiment text` — Returns "positive", "neutral", or "negative"
- `batch_sentiment texts` — Batch-classify multiple texts
- `sentiment_report texts` — Returns count map of sentiment categories

## Usage

```pipe
import "https://raw.githubusercontent.com/MachuraHarry/pipe-modules/main/sentiment/module.pipe"

tweets: read_lines "tweets.txt"
report: sentiment_report tweets
```
