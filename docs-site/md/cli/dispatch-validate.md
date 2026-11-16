# pso dispatch:validate

> Statically validate event declarations, schemas, consumers, providers and the contract lock (exit 1 on errors)

Statically validate event declarations, schemas, consumers, providers and the contract lock (exit 1 on errors).

## Usage

```text
pso dispatch:validate [--json]
```

## Flags

`--json`.

## Examples

```bash
pso dispatch:validate
pso dispatch:validate --json
```

## Failure cases

- Exit 1 on invalid declarations, schemas, consumer registrations, provider consistency or lock drift. No broker is contacted.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [dispatch/contracts](dispatch/contracts.md)
