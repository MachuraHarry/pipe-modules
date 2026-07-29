# parallel-runner

Run multiple functions or AI queries in parallel using Pipe's `>>` operator.

## Functions

- `ask_many questions` — Run multiple AI questions in parallel, returns list of answers
- `summarize_many texts` — Summarize multiple texts in parallel
- `translate_many texts target` — Translate multiple texts to target language in parallel

## Usage

```pipe
ai_provider "deepseek"

import "parallel-runner@1.0.0"

fragen: [
    "Was ist die Hauptstadt von Frankreich?"
    "Wie hoch ist der Mount Everest?"
    "Wieviel ist 7 * 8 + 4?"
]

antworten: ask_many fragen
for a in antworten
    print a
```

Each AI query starts in the background. Results auto-resolve when accessed — no manual synchronization needed.

## Install

```bash
pipe -get parallel-runner
pipe -get parallel-runner@1.0.0
```
