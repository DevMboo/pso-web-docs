# pso dispatch:inspect

> Show the declared dispatch setup of each service: provider, enabled state, events, consumers and settings (secrets redacted; no broker is contacted)

Show the declared dispatch setup of each service: provider, enabled state, events, consumers and settings (secrets redacted; no broker is contacted).

## Usage

```text
pso dispatch:inspect [--service <name>] [--json]
```

## Flags

`--service`, `--json`.

## Examples

```bash
pso dispatch:inspect
pso dispatch:inspect --service orders --json
```

## Failure cases

- This is a static view with secrets redacted; use `dispatch:doctor` for broker reachability.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [dispatch/installation](dispatch/installation.md)
