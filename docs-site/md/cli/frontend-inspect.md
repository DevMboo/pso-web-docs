# pso frontend:inspect

> Show a frontend project: identity, port, orchestrator, registered MEIs, middleware order, scopes and effective routes

Show a frontend project: identity, port, orchestrator, registered MEIs, middleware order, scopes and effective routes.

## Usage

```text
pso frontend:inspect [<project>] [--json]
```

## Flags

`--json`.

## Examples

```bash
pso frontend:inspect
pso frontend:inspect fei-commerce
pso frontend:inspect customers --json
```

## What it changes

- Prints identity, port, orchestrator, middleware order, mounts and effective routes of a project.

## Failure cases

- `Unknown frontend project`: names the known projects.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [frontend/declarations](frontend/declarations.md)
