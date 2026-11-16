# pso graph

> Show service dependencies and orchestrator routes together

Show service dependencies and orchestrator routes together.

## Usage

```text
pso graph [--mermaid] [--json]
```

## Flags

`--mermaid`, `--json`.

## Examples

```bash
pso graph
pso graph --mermaid
pso graph --json
```

## Failure cases

- This combines declared service dependencies and gateway routes; use `services:health` to test availability.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [tutorials/multiple-services](tutorials/multiple-services.md)
