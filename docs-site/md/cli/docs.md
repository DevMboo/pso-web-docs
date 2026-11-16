# pso docs

> Serve the documentation portal locally, or write the complete static site (search, llms.txt, AI context) with --build

Serve the documentation portal locally, or write the complete static site (search, llms.txt, AI context) with --build.

## Usage

```text
pso docs [--port N] [--no-open] [--repository <https-url>] | pso docs --build <dir> [--repository <https-url>] [--json]
```

## Flags

| Flag | What it does |
|---|---|
| `--port` | Choose the local documentation server port. |
| `--no-open` | Print the URL without launching a browser. |
| `--repository` | Add the HTTPS source repository link to the portal. |
| `--build` | Write a static site to the given directory instead of serving it. |
| `--json` | See the usage line and the task guide for this option. |

## Examples

```bash
pso docs
pso docs --build ./site
```

## What it changes

- Without `--build`, serves a temporary local portal.
- With `--build`, writes HTML, search data, Markdown and AI context files under the selected directory.

## Verify the result

- Open the printed local URL, or open `site/index.html` after a static build.
- Check a guide page and its linked example assets before publishing the site.

## Failure cases

- `--build` writes a complete static site, including search, `llms.txt` and the AI context files.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
