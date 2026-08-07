# pipe-cli

Lightweight CLI framework for Pipe scripts. Subcommands, typed flags, auto-generated `--help`.

## Functions

- `app(name, desc)` — create a CLI app
- `command(app, name, desc)` — register a subcommand
- `flag(cmd, name, short, desc, default)` — add a flag (string, bool, int)
- `handler(cmd, fn)` — set the command handler
- `run(app, args)` — parse args, route to handler, generate help

## Usage

```pipe
import "pipe-cli"

fn build_h opts
    print ("Building -> " ++ (get opts "output"))
    if (get opts "verbose")
        print "Verbose mode on"

cli: app "mytool" "My project tool"

cmd: command cli "build" "Compile project"
flag cmd "output" "o" "Output directory" "dist/"
flag cmd "verbose" "v" "Verbose output" false
handler cmd build_h

run cli args
```

```bash
$ pipe mytool.pipe build --output out/ --verbose
Building -> out/
Verbose mode on

$ pipe mytool.pipe build --help
Options for build:

  --output, -o    Output directory (default: dist/)
  --verbose, -v    Verbose output (default: false)

$ pipe mytool.pipe
mytool — My project tool

Usage: pipe <file> <command> [options]

Commands:
  build    Compile project
```

## Flag Types

| Default value | Type | Parsing |
|--------------|------|---------|
| `"string"` | String | `--flag value` |
| `false` | Boolean | `--flag` sets to true |
| `42` | Integer | `--flag 123` |

Handler receives a map: `{flag_name: value, ...}`.
