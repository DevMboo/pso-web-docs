# pso services:unregister

> Remove a service declaration (refused while others depend on it; files are never deleted)

Remove a service declaration (refused while others depend on it; files are never deleted).

## Usage

```text
pso services:unregister <name> [--dry-run] [--json]
```

## Flags

`--dry-run`, `--json`.

## Examples

```bash
pso services:unregister catalog --dry-run
pso services:unregister catalog
```

## Failure cases

- A service with declared dependents cannot be removed first. Project files are never deleted.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [microservices/service-registration](microservices/service-registration.md)
