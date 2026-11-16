# pso dispatch:doctor

> Validate and then really connect to each configured broker; reports up, down or not configured (never claims a connection from configuration alone)

Validate and then really connect to each configured broker; reports up, down or not configured (never claims a connection from configuration alone).

## Usage

```text
pso dispatch:doctor [--service <name>] [--json]
```

## Flags

`--service`, `--json`.

## Examples

```bash
pso dispatch:doctor --service orders
pso dispatch:doctor --json
```

## Failure cases

- This command really connects to the configured broker. A down result means configuration or connectivity needs investigation.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [dispatch/installation](dispatch/installation.md)
