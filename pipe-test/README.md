# pipe-test

Better assertions for Pipe's built-in `test` blocks. Drop-in enhancement — works with `pipe -test`.

## Functions

**Equality:**
- `expect_eq(actual, expected, msg?)` — assert equality with custom message
- `expect_ne(actual, expected, msg?)` — assert inequality

**Truthiness:**
- `expect_truthy(val, msg?)` — assert value is truthy
- `expect_falsy(val, msg?)` — assert value is falsy

**Nil checks:**
- `expect_nil(val, msg?)` — assert value is nil
- `expect_not_nil(val, msg?)` — assert value is not nil

**Numeric comparisons:**
- `expect_gt(actual, expected, msg?)` — assert actual > expected
- `expect_lt(actual, expected, msg?)` — assert actual < expected
- `expect_gte(actual, expected, msg?)` — assert actual >= expected
- `expect_lte(actual, expected, msg?)` — assert actual <= expected

**Containment:**
- `expect_contains(container, item, msg?)` — assert list/string contains item

**Type checks:**
- `expect_type(val, type_name, msg?)` — assert type_of(val) == type_name

## Usage

```pipe
import "https://raw.githubusercontent.com/MachuraHarry/pipe-modules/master/pipe-test/module.pipe"

test "basic assertions"
    expect_eq (2 + 2) 4 "math works"
    expect_truthy (len [1, 2, 3]) "non-empty list"
    expect_contains ["a", "b", "c"] "b" "found b"
    expect_type 42 "INTEGER" "type check"

test "custom failure message"
    -- If this fails, prints:
    -- FAIL ... (expected 5 but got 4 — wrong answer)
    expect_eq (2 + 2) 4 "wrong answer"
```

Run with: `pipe -test`
