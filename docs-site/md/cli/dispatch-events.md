# pso dispatch:events

> List every declared event with its version, channel, owner service, publishers and consumers

List every declared event with its version, channel, owner service, publishers and consumers.

## Usage

```text
pso dispatch:events [--json]
```

## Flags

`--json`.

## Examples

```bash
pso dispatch:events
pso dispatch:events --json
```

## Failure cases

- The list comes from declarations, not messages currently held by a broker.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [dispatch/events-and-consumers](dispatch/events-and-consumers.md)
