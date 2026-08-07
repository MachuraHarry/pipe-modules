# Contributing to Pipe Modules

Anyone can contribute a module. Here's how.

## How the ecosystem works

```
1. You write a   2. You push it   3. Anyone finds   4. They use it
   module.pipe      to this repo     it via            via import
                   (via PR)          pipe -search      or pipe -get
```

## Quick Start (5 minutes)

1. Fork this repo
2. Scaffold a module: `pipe -init my-module`
3. Edit `my-module/pipe.json` — update description, add exports
4. Edit `my-module/module.pipe` — write your code
5. Validate: `pipe -validate my-module`
6. Open a Pull Request

### pipe.json Format

Every module needs a `pipe.json` manifest:

```json
{
  "name": "my-module",
  "version": "0.1.0",
  "description": "What it does",
  "author": "your-name",
  "license": "MIT",
  "exports": ["my_function", "another_fn"],
  "dependencies": {
    "pipe-http": "^1.0.0"
  }
}
```

- **name** (required): lowercase letters, digits, hyphens, underscores
- **version** (required): semver-compatible version string
- **exports** (optional): list of function names. Helps with discovery and LSP.
- **dependencies** (optional): other modules your module needs

### Example Module

- **One folder per module.** Name it after the module (e.g., `log-analyzer/`)
- **Export your functions.** Use `export fn` so they're visible on import
- **Keep it focused.** One module = one job. Don't make mega-modules.
- **No API keys in code.** Use environment variables (users set their own).
- **Test before submitting.** Run `pipe -validate .` to check syntax and manifest.
- **Include a pipe.json.** All modules must have a valid pipe.json manifest.

## Example Module

```
my-module/
├── pipe.json          ← Module manifest (required)
├── module.pipe        ← Your code (required)
└── README.md          ← What it does (recommended)
```

## Registration

`registry.json` is **auto-generated** from `pipe.json` files. When your PR is merged:

1. A maintainer runs `pipe -gen-registry .` to update `registry.json`
2. Your module appears in `pipe -search` immediately
3. Users can install it with `pipe -get your-module`
4. Or they add it to their `pipe.json` dependencies and run `pipe -install`

### Adding Dependencies

If your module depends on others, list them in `pipe.json`:

```json
{
  "name": "my-module",
  "version": "0.1.0",
  "dependencies": {
    "pipe-http": "^1.0.0",
    "sqlite": "^0.8.0"
  }
}
```

Users install all dependencies recursively with:
```
pipe -install
```

You don't need to edit `registry.json` yourself — it's generated from your pipe.json.

## Review Process

- PRs are reviewed by a maintainer
- If your module has valid Pipe syntax and does something useful, it gets merged
- Typical review time: 1-3 days
- You don't need permission — just open a PR

## Requirements

- Must be valid Pipe syntax (run `pipe -ast module.pipe`)
- Must export at least one function
- Must have a clear purpose
- No malicious code (obviously)

## Questions?

Open an issue in this repo.
