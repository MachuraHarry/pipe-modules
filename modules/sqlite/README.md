# sqlite

Pure-Pipe relational database engine. Implements a SQLite-compatible SQL subset entirely in Pipe, with no external C dependencies.

## Features

- DDL: CREATE TABLE, DROP TABLE, CREATE INDEX
- DML: INSERT, UPDATE, DELETE
- SELECT with WHERE, GROUP BY, ORDER BY, LIMIT/OFFSET, DISTINCT, JOINs (INNER/LEFT/RIGHT)
- Aggregate functions: COUNT, SUM, AVG, MIN, MAX
- Transactions: BEGIN, COMMIT, ROLLBACK
- Binary persistence via paged file I/O with CRC32 checksums
- In-memory databases (`":memory:"`)

## Functions

- `db_open(path)` — Open a database file or `":memory:"`, returns integer handle
- `db_close(handle)` — Close database, persist to disk, release handle
- `db_exec(handle, sql)` — Execute DDL/DML statements, returns affected row count
- `db_query(handle, sql)` — Execute SELECT query, returns list of row maps
- `q(handle, sql)` — Short alias for `db_query`, pipeline-friendly
- `exec(handle, sql)` — Short alias for `db_exec`
- `row_get(row, key)` — Nil-safe field access from a row map
- `row_eq(row, key, val)` — Predicate: row[key] == val, for `filter`
- `row_ne(row, key, val)` — Predicate: row[key] != val, for `filter`

## Usage

```pipe
import "https://raw.githubusercontent.com/MachuraHarry/pipe-modules/master/sqlite/module.pipe"

h: db_open "mydata.db"
db_exec h "CREATE TABLE users (id INTEGER PRIMARY KEY, name TEXT, age INTEGER)"
db_exec h "INSERT INTO users VALUES (1, 'Alice', 30)"
rows: db_query h "SELECT * FROM users WHERE age > 25"
db_close h

-- Pipeline style
db_open "mydata.db" > q "SELECT * FROM users" > filter (fn r -> row_gt r "age" 25) > each print
```

## Status

Fully functional in tree-walker (TV) mode. VM mode has a known compiler bug with large module imports — tracked in the main Pipe repository.
