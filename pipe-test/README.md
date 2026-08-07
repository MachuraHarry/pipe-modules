# pipe-test

Better assertions and test hooks for Pipe's built-in `test` blocks. Works with `pipe -test`.

## Functions

**Equality:**
- `expect_eq(actual, expected, msg?)` — assert equality
- `expect_ne(actual, expected, msg?)` — assert inequality

**Truthiness:**
- `expect_truthy(val, msg?)` — assert value is truthy
- `expect_falsy(val, msg?)` — assert value is falsy

**Nil checks:**
- `expect_nil(val, msg?)` — assert value is nil
- `expect_not_nil(val, msg?)` — assert value is not nil

**Numeric comparisons:**
- `expect_gt(a, b, msg?)` / `expect_lt` / `expect_gte` / `expect_lte`

**Containment:**
- `expect_contains(container, item, msg?)` — list or string contains

**Type checks:**
- `expect_type(val, type_name, msg?)` — assert type_of

**Error catching (requires Pipe v0.8+ with fixed try/catch):**
- `expect_error(fn)` — calls fn, returns caught error message. Fails if no error.
- `expect_no_error(fn)` — calls fn, fails if an error occurs.

**Hooks:**
- `with_hooks(before_fn, test_fn, after_fn)` — run before → test → after sequence

> Note: hook and test functions must accept one dummy argument (`fn my_fn _`).

## Usage

```pipe
import "pipe-test"

-- Error testing
fn bad_db _
    raise "connection refused"

test "error handling"
    err: expect_error bad_db
    expect_contains err "connection" "right error"

-- Hooks
test "setup and teardown"
    state: [0]
    fn setup _
        set state 0 99
    fn check _
        expect_eq (at state 0) 99 "setup ran"
    fn teardown _
        set state 0 0
    with_hooks setup check teardown
```

Run with: `pipe -test`
