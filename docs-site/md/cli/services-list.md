# pso services:list

> List services declared in the workspace

List services declared in the workspace.

## Usage

```text
pso services:list [--json]
```

## Flags

`--json`.

## Examples

```bash
pso services:list
pso services:list --json
```

## Failure cases

- Registration does not imply that a service is running; use `pso services:health` for a live probe.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [microservices/service-registration](microservices/service-registration.md)
