# pso make:module

> Create an HTTP module: MODULE.pso.ts + an empty ROUTES.pso.ts (no endpoint is exposed yet)

Create an HTTP module: MODULE.pso.ts + an empty ROUTES.pso.ts (no endpoint is exposed yet).

## Usage

```text
pso make:module <name> [--service <orchestrator>] [--dry-run] [--json]
```

## Flags

`--service`, `--dry-run`, `--json`.

## Examples

```bash
pso make:module users
```

## What it changes

- Creates an empty module declaration and route registry under `src/routes/users/`.
- No HTTP endpoint exists until a compatible controller action is declared in the registry.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
created  src/routes/users/MODULE.pso.ts
created  src/routes/users/ROUTES.pso.ts
```

## Verify the result

- Open both generated files, then run `pso validate`.
- Use `pso routes:list` after adding a route; an empty module contributes no public URL.

## Failure cases

- `health` is reserved by the framework.
- An existing module is never overwritten.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [orchestrator/module](orchestrator/module.md)
