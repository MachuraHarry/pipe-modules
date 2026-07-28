# code-review

AI-powered code review.

## Functions

- `review_code code` — Detailed code review with suggestions
- `rate_code code` — Rate code 1-10

## Usage

```pipe
import "https://raw.githubusercontent.com/MachuraHarry/pipe-modules/main/code-review/module.pipe"

read_file "src/main.go" > review_code > save "review.md"
```
