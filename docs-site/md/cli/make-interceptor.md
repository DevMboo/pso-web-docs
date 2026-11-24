# pso make:interceptor

> Generate an interceptor in src/.app/interceptors/ (modular orchestrator). It runs around the action of the routes that name it (interceptors: [...] in ROUTES.pso.ts)

Generate an interceptor in src/.app/interceptors/ (modular orchestrator). It runs around the action of the routes that name it (interceptors: [...] in ROUTES.pso.ts).

## Usage

```text
pso make:interceptor <name> [--service <name>] [--dry-run] [--force] [--json]
```

## Flags

`--service`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso make:interceptor timing
pso make:interceptor audit --dry-run
```

## What it changes

- Writes `src/.app/interceptors/<name>.interceptor.ts`; a route opts in with `interceptors: ["<name>"]`.

## Failure cases

- `needs the modular structure`: 0.1 orchestrators have no interceptors directory; see `pso structure:inspect`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [architecture/typescript-orchestrator](architecture/typescript-orchestrator.md)
