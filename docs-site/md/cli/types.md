# pso types

> Write src/pso-types.d.ts in the orchestrator: the registered service names and the guard names as TypeScript types, so the compiler checks them wherever they are named (--check: exit 1 when the file is missing or out of date)

Write src/pso-types.d.ts in the orchestrator: the registered service names and the guard names as TypeScript types, so the compiler checks them wherever they are named (--check: exit 1 when the file is missing or out of date).

## Usage

```text
pso types [--check] [--dry-run] [--json]
```

## Flags

`--check`, `--dry-run`, `--json`.

## Examples

```bash
pso types
pso types --check
pso types --dry-run
```

## What it changes

- Writes `src/pso-types.d.ts` in the orchestrator: the services registered in `pso.workspace.json` and the guards of the project as TypeScript types.
- From then on the compiler refuses a service or guard name that does not exist in `ROUTES.pso.ts`, `Upstream.forward` and `UpstreamClient`. Without the file every string is accepted, as before.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
created    gateway/src/pso-types.d.ts
services: catalog, customers, orders
guards:   jwt
```

## Verify the result

- `npm run typecheck` in the orchestrator.
- `pso types --check` exits 1 when the file is missing or out of date (for a pipeline).

## Failure cases

- Run it again after `pso create service`, `pso services:register` or `pso make:guard`: the file is generated, never edited by hand.
- The run-time checks do not depend on it: `pso validate` and start-up refuse an unknown service or guard either way.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [orchestrator/routes](orchestrator/routes.md)
- [orchestrator/upstream-clients](orchestrator/upstream-clients.md)
