# pipe-date

DateTime utilities for Pipe — parsing, formatting, arithmetic, comparisons.

Requires Pipe v0.8+ (`parse_date` builtin).

## Functions

**Parsing & Formatting:**
- `parse(date_str, layout?)` — parse date string to Unix timestamp
- `format(ts, layout)` — format timestamp to string
- `now_iso(_)` — current time in ISO 8601

**Date Parts:**
- `part(ts, part_name)` — extract year, month, day, hour, minute, second, weekday

**Arithmetic:**
- `add_days(ts, days)` / `add_hours(ts, hours)` / `add_minutes(ts, mins)`
- `diff_days(ts1, ts2)` / `diff_hours(ts1, ts2)`

**Comparison:**
- `is_before(ts1, ts2)` / `is_after(ts1, ts2)`
- `is_between(ts, start, end)`

**Checks:**
- `is_weekend(ts)` / `is_weekday(ts)` / `is_today(ts)`

**Relative:**
- `relative(ts)` — "3 days ago" / "in 2 hours"

## Usage

```pipe
import "pipe-date"

-- Parse and format
ts: parse "2024-03-15" "2006-01-02"
print (format ts "January 2, 2006")    -- March 15, 2024

-- Arithmetic
later: add_days ts 7
print (format later "2006-01-02")      -- 2024-03-22

-- Parts
year: part ts "year"                    -- 2024
month: part ts "month"                  -- 3
wd: part ts "weekday"                   -- 5 (= Friday)

-- Relative time
past: now - 86400
print (relative past)                   -- 1 day ago

-- Weekends
sat: parse "2024-03-16" "2006-01-02"
print (is_weekend sat)                 -- true
```
