# pso trust:list

> Show which signers the environment's server trusts (and which were revoked or expired)

Show which signers the environment's server trusts (and which were revoked or expired).

## Usage

```text
pso trust:list <environment> [--json]
```

## Flags

`--json`.

## Examples

```bash
pso trust:list production
```

## Failure cases

- Needs a session on the environment; revoked and expired signers are shown as such.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
