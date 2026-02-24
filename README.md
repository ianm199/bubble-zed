# Bubble for Zed

See exception flow through your Python codebase directly in Zed.

## What it does

Hover over Python code to see exception information:

- **`def` lines**: Shows all exceptions that can escape from the function (uncaught, framework-handled, caught locally, caught by global handler)
- **Function calls**: Shows what exceptions the callee can throw
- **Everything else**: No popup (no noise)

## Requirements

Install [bubble-analysis](https://pypi.org/project/bubble-analysis/) with LSP support in your project's Python environment:

```bash
pip install bubble-analysis[lsp]
```

The extension finds `python3` on your PATH and runs `python -m bubble.lsp`.

## Configuration

Override the Python binary or arguments in your Zed settings (`settings.json`):

```json
{
  "lsp": {
    "bubble-lsp": {
      "binary": {
        "path": "/path/to/your/venv/bin/python",
        "arguments": ["-m", "bubble.lsp"]
      }
    }
  }
}
```

## How it works

The extension spawns a Python LSP server that:

1. Builds a program model of your codebase on first hover
2. Propagates exceptions through the call graph (cached after first run)
3. Returns context-sensitive hover info based on cursor position

The model rebuilds automatically when you save a file.

## Supported frameworks

Exception flow analysis supports Flask, FastAPI, and CLI scripts. Framework-specific exceptions (like `HTTPException`) are recognized and categorized appropriately.

## Publishing updates

This repo is registered as a submodule in [zed-extensions](https://github.com/zed-industries/extensions) under `extensions/bubble`. A canonical copy also lives in the main bubble-analysis repo at `editors/zed/`.

To publish a new version:

1. Make changes here and push to `main`.
2. In your fork of `zed-extensions`, update the submodule pointer:
   ```bash
   cd extensions/bubble
   git pull origin main
   cd ../..
   ```
3. Bump the version in `extension.toml` here **and** in `extensions.toml` in zed-extensions (under `[bubble-lsp]`).
4. Commit the submodule update and version bump, then open a PR against `zed-industries/extensions`.
5. Sync any changes back to `editors/zed/` in the bubble-analysis repo to keep the two copies in lockstep.
