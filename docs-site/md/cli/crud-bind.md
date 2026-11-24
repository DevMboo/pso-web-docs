# pso crud:bind

> Bind a CRUD module of the orchestrator to the microservice that serves its resource (the <R>_SERVICE contract constant and the metadata.services of its routes, together)

Bind a CRUD module of the orchestrator to the microservice that serves its resource (the <R>_SERVICE contract constant and the metadata.services of its routes, together).

## Usage

```text
pso crud:bind <module> --to <service> [--json]
```

## Flags

`--to`, `--json`.

## Examples

```bash
pso crud:bind products --to catalog
pso crud:bind orders --to sales --json
```

## What it changes

- Rewrites the module's `<R>_SERVICE` contract constant and the `metadata.services` of its routes together.

## Failure cases

- `is not a service of this workspace`: create or register the service first.
- `is not a CRUD module`: only modules with a `contracts/*.contract.ts` binding constant can be rebound.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [architecture/reference-crud](architecture/reference-crud.md)
