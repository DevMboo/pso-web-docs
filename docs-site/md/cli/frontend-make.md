# pso frontend:make

> Generate a navigation middleware (FEI: src/middlewares, MEI: domain middleware) or add a route with its page to ROUTES.pso.ts

Generate a navigation middleware (FEI: src/middlewares, MEI: domain middleware) or add a route with its page to ROUTES.pso.ts.

## Usage

```text
pso frontend:make middleware|route <name> [--project <fei-or-mei>] [--page ./src/pages/X.tsx] [--dry-run] [--force]
```

## Flags

`--project`, `--page`, `--dry-run`, `--force`.

## Examples

```bash
pso frontend:make middleware maintenance-window --project fei-shop
pso frontend:make route /new --project mei-customers --dry-run
```

## What it changes

- `middleware` writes `src/middlewares/<name>.ts` and lists it in the declaration (the FEI: last in the pipeline, move it where it belongs). `route` writes a page and adds the route to `ROUTES.pso.ts`.

## Failure cases

- `is a built-in middleware name`: choose another.
- `Several frontend projects exist`: pass `--project`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [frontend/routing-and-middleware](frontend/routing-and-middleware.md)
