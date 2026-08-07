# jpipe

JSON path query module for Pipe — navigate, pick, and flatten JSON/Map structures with a concise path syntax.

## Functions

- `jp(obj, path)` — Evaluate a path expression against an object
- `jpick(obj, keys)` — Pick specific fields from a map
- `jkeys(obj)` — List all leaf key paths (recursive)
- `jflatten(obj)` — Flatten nested structure to `{key.path: value}`

## Path Syntax

| Expression | Meaning |
|-----------|---------|
| `.name` | Access map key `name` |
| `.users[0]` | Access array index 0 |
| `.users[0].name` | Chain: nested access |
| `.users[*].name` | Wildcard: all elements |
| `.a.b.c` | Deep nesting |

## Usage

```pipe
import "https://raw.githubusercontent.com/MachuraHarry/pipe-modules/master/jpipe/module.pipe"

data: parse_json "{\"users\": [{\"name\": \"Alice\", \"age\": 30}, {\"name\": \"Bob\", \"age\": 25}]}"

-- Navigate to a specific value
name: jp data ".users[0].name"           -- "Alice"

-- Get all names (wildcard)
names: jp data ".users[*].name"           -- ["Alice", "Bob"]

-- Pick specific fields
u: at (jp data ".users") 0
fields: ["name", "age"]
picked: jpick u fields                    -- {name: "Alice", age: 30}

-- List all leaf paths
keys: jkeys data
-- ["users[0].name", "users[0].age", "users[1].name", "users[1].age"]

-- Flatten to key-value pairs
flat: jflatten data
-- {"users[0].name": "Alice", "users[0].age": 30, ...}
```
