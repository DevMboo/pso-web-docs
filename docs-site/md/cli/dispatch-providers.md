# pso dispatch:providers

> List the supported dispatch providers, their guarantees and which service has which adapter installed

List the supported dispatch providers, their guarantees and which service has which adapter installed.

## Usage

```text
pso dispatch:providers [--json]
```

## Flags

`--json`.

## Examples

```bash
pso dispatch:providers
pso dispatch:providers --json
```

## Failure cases

- Capabilities are adapter metadata, not proof that a broker is currently available.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [dispatch/providers](dispatch/providers.md)
