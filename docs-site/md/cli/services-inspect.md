# pso services:inspect

> Show declaration, discovered resources and live health of one service

Show declaration, discovered resources and live health of one service.

## Usage

```text
pso services:inspect <name> [--json]
```

## Flags

`--json`.

## Examples

```bash
pso services:inspect catalog
pso services:inspect gateway --json
```

## Failure cases

- An unknown service name is rejected; list registered names with `pso services:list`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [microservices/service-registration](microservices/service-registration.md)
