# pso environment:list

> List the environments declared in this project (deploy/*.deployment.pso.ts) with their connection, strategy and declaration problems

List the environments declared in this project (deploy/*.deployment.pso.ts) with their connection, strategy and declaration problems.

## Usage

```text
pso environment:list [--json]
```

## Flags

`--json`.

## Examples

```bash
pso environment:list
pso environment:list --json
```

## Failure cases

- A declaration with errors is listed with its problem count; run pso pipeline:validate to see each one. A connection reported as missing has not been added on this machine.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/environments](cloud/environments.md)
