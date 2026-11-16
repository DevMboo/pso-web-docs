# pso dispatch:consumers

> List every declared consumer (event, handler, service, group)

List every declared consumer (event, handler, service, group).

## Usage

```text
pso dispatch:consumers [--json]
```

## Flags

`--json`.

## Examples

```bash
pso dispatch:consumers
pso dispatch:consumers --json
```

## Failure cases

- A decorated TypeScript consumer absent from `EVENTS.pso.ts` is not active.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [dispatch/events-and-consumers](dispatch/events-and-consumers.md)
