# pso routes:list

> List the routes declared in ROUTES.pso.ts files with their effective policy; fails when the registry is invalid

List the routes declared in ROUTES.pso.ts files with their effective policy; fails when the registry is invalid.

## Usage

```text
pso routes:list [--json] [--out <file>]
```

## Flags

`--json`, `--out`.

## Examples

```bash
pso routes:list
pso routes:list --json
```

## What it changes

- Reads declared gateway routes; it does not send a request to the upstream service.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
GET     /customers     customer.findAll   -> users   guard:jwt
```

## Verify the result

- Use `pso routes:inspect customer` for the exact policy and target, then call the URL to verify runtime behavior.

## Failure cases

- Exit code 1 and rows marked `[NOT ACTIVE]` when the route registry fails validation: no route is active in that state.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [orchestrator/routes](orchestrator/routes.md)
