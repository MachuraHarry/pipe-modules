# Pipe Modules — Curated Pipeline Library

A collection of reusable Pipe (SPR) modules for the Semantic Pipeline Runtime.

## Usage

```pipe
import "https://raw.githubusercontent.com/MachuraHarry/pipe-modules/main/log-analyzer/module.pipe"

read_file "errors.log" > log_analyze > save "report.md"
```

## Install via CLI

```bash
pipe -get log-analyzer
pipe -get https://raw.githubusercontent.com/MachuraHarry/pipe-modules/main/log-analyzer/module.pipe
```

## Available Modules

| Module | Description |
|--------|-------------|
| `log-analyzer` | Classify, summarize, and report on log files |
| `sentiment` | Analyze sentiment of texts |
| `code-review` | AI-powered code review |
| `translate` | Batch translation to multiple languages |
| `incident-report` | Security incident analysis & reporting |
| `summary` | Text summarization utilities |

## Module Structure

Each module is a self-contained `.pipe` file with exported functions.

```
pipe-modules/
├── log-analyzer/
│   ├── module.pipe
│   └── README.md
├── sentiment/
│   ├── module.pipe
│   └── README.md
└── registry.json
```

## Contributing

1. Create a folder with your module name
2. Add `module.pipe` with exported functions
3. Open a PR
