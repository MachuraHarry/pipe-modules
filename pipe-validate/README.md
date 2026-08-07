# pipe-validate

Schema validation for Pipe maps — type checking, required fields, numeric ranges, regex patterns, allowed values.

## Functions

- `validate(data, schema)` — validates data against schema, returns data on success, raises on first error
- `is_valid(data, schema)` — returns true/false without raising

## Schema Format

A list of field definitions:
```pipe
schema: []
f: {}; set f "field" "name"; set f "type" "string"; set f "required" true; push schema f
```

**Constraints per field:**
| Key | Type | Description |
|-----|------|-------------|
| `field` | string | Field name (required) |
| `type` | string | `string`, `number`, `boolean`, `list`, `map`, `any` |
| `required` | boolean | Must be present and non-nil |
| `min` / `max` | number | Numeric range |
| `min_len` / `max_len` | number | String/list length range |
| `regex` | string | Pattern match for strings |
| `values` | list | Allowed values |

## Usage

```pipe
import "pipe-validate"

schema: []
f1: {}; set f1 "field" "email"; set f1 "type" "string"
set f1 "required" true; set f1 "regex" ".+@.+"; push schema f1
f2: {}; set f2 "field" "age"; set f2 "type" "number"
set f2 "min" 0; set f2 "max" 150; push schema f2

data: {}
set data "email" "user@example.com"
set data "age" 30

validate data schema              -- passes
is_valid data schema              -- true

-- Pipeline style
data > validate schema > process
```
