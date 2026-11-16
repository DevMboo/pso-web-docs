# pso release:recover

> Reconcile an interrupted deployment: inspects what really exists on the server, then completes it, undoes it, or stops and asks for an operator when the state is ambiguous. Never replays steps blindly

Reconcile an interrupted deployment: inspects what really exists on the server, then completes it, undoes it, or stops and asks for an operator when the state is ambiguous. Never replays steps blindly.

## Usage

```text
pso release:recover <environment> --yes [--json]
```

## Flags

`--yes`, `--json`.

## Examples

```bash
pso release:recover production --yes
```

## What it changes

- Inspects the real state (nginx's live release marker, processes, health) and completes forward, undoes, or stops with `needs-operator`.

## Failure cases

- `needs-operator`: the state is ambiguous; nothing was changed and a person must decide.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/rollback-and-recovery](cloud/rollback-and-recovery.md)
