# pso routes:inspect

> Show one orchestrator route in full: target, access policy, contract and the service-side handler

Show one orchestrator route in full: target, access policy, contract and the service-side handler.

## Usage

```text
pso routes:inspect <id-or-path> [--json]
```

## Flags

`--json`.

## Examples

```bash
pso routes:inspect customer
pso routes:inspect /customers --json
```

## Failure cases

- `No route`: use a module name, a route id (`module.action`) or an exact path.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
