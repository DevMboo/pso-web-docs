# pso release:cancel

> Cancel a running deployment at the next safe point (before traffic moves); the active release is untouched. After traffic moved, use rollback

Cancel a running deployment at the next safe point (before traffic moves); the active release is untouched. After traffic moved, use rollback.

## Usage

```text
pso release:cancel <environment> --deployment <id> --yes [--json]
```

## Flags

`--deployment`, `--yes`, `--json`.

## Examples

```bash
pso release:cancel production --deployment d1abc... --yes
```

## Failure cases

- After traffic moved, cancelling is refused: use rollback.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/rollback-and-recovery](cloud/rollback-and-recovery.md)
