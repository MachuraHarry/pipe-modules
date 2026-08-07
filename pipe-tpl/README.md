# pipe-tpl

Template engine for Pipe with `{{ variable }}`, `{{ if }}`, `{{ for }}`, and filters.

## Syntax

```
{{ name }}                  — variable lookup
{{ user.name }}             — nested path (dot notation)
{{ name | upper }}          — pipe filter
{{ if show }}...{{ end }}   — conditional block
{{ if x }}A{{ else }}B{{ end }} — if/else
{{ for item in list }}...{{ end }} — iteration
{{-- comment --}}           — comment
```

## Filters

| Filter | Description |
|--------|-------------|
| `upper` | Uppercase |
| `lower` | Lowercase |
| `len` | Length |
| `to_str` | Convert to string |
| `keys` | Map keys |
| `type` | Type name |

## Usage

```pipe
import "pipe-tpl"

data: {name: "Alice", items: [1, 2, 3], active: true}

render "Hello {{ name }}! Items: {{ items | len }}" data
-- Hello Alice! Items: 3

-- From file
render_file "report.md.tpl" data

-- With conditionals and loops
tpl: "{{ if active }}Active user: {{ name }}{{ end }}
{{ for item in items }}- {{ item }}
{{ end }}"
render tpl data
```
