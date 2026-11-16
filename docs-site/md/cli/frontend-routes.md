# pso frontend:routes

> List the effective browser routes of the whole frontend ecosystem with their owners (exit 1 on conflicts)

List the effective browser routes of the whole frontend ecosystem with their owners (exit 1 on conflicts).

## Usage

```text
pso frontend:routes [--json]
```

## Flags

`--json`.

## Examples

```bash
pso frontend:routes
pso frontend:routes --json
```

## What it changes

- Lists every effective browser route of the ecosystem with its owner, target and scopes, sorted by path.

## Failure cases

- Exit code 1 when two projects (or one) declare the same effective path.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [frontend/routing-and-middleware](frontend/routing-and-middleware.md)
