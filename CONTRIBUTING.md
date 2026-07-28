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
2. Create a folder: `mkdir my-module`
3. Write a `module.pipe` file:

```pipe
export fn my_function input
    ask ("Process this: " ++ input)
```

4. Create a `README.md` explaining what it does
5. Open a Pull Request

## Module Rules

- **One folder per module.** Name it after the module (e.g., `log-analyzer/`)
- **Export your functions.** Use `export fn` so they're visible on import
- **Keep it focused.** One module = one job. Don't make mega-modules.
- **No API keys in code.** Use environment variables (users set their own).
- **Test before submitting.** Run `pipe -ast module.pipe` to check syntax.

## Example Module

```
my-module/
├── module.pipe       ← Your code (required)
└── README.md         ← What it does (optional but recommended)
```

## Registration

After your PR is merged, maintainers update `registry.json` with your module name.
You don't need to edit `registry.json` yourself — we handle that.

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
