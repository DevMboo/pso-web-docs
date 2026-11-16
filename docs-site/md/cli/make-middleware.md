# pso make:middleware

> Generate a middleware in src/middlewares/ (orchestrator). It runs only for the routes that name it in ROUTES.pso.ts

Generate a middleware in src/middlewares/ (orchestrator). It runs only for the routes that name it in ROUTES.pso.ts.

## Usage

```text
pso make:middleware <name> [--service <name>] [--dry-run] [--force] [--json]
```

## Flags

`--service`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso make:middleware correlation --service gateway --dry-run
pso make:middleware correlation --service gateway
```

## Failure cases

- A generated middleware file must be referenced by a route or module before it runs.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [orchestrator/middleware-and-filters](orchestrator/middleware-and-filters.md)
