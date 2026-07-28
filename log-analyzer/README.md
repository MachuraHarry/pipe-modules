# log-analyzer

Classify, summarize, and report on log files using AI.

## Functions

- `log_analyze logs` — Analyze a list of log lines, returns summary string
- `log_classify logs` — Classify each log line, returns list of categories

## Usage

```pipe
import "https://raw.githubusercontent.com/MachuraHarry/pipe-modules/main/log-analyzer/module.pipe"

read_file "errors.log" > split "\n" > log_analyze > save "report.md"
```
