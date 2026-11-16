# pso release:rollback

> Re-activate the previous (or a chosen kept) release with the same candidate-verify-switch-drain sequence. Application rollback does not undo database changes

Re-activate the previous (or a chosen kept) release with the same candidate-verify-switch-drain sequence. Application rollback does not undo database changes.

## Usage

```text
pso release:rollback <environment> [--to <release>] --yes [--no-follow] [--json]
```

## Flags

`--to`, `--yes`, `--no-follow`, `--json`.

## Examples

```bash
pso release:rollback production --yes
pso release:rollback production --to r20260101120000-ab12cd34 --yes
```

## Failure cases

- Application rollback does not undo database changes; the plan said so before the deployment.
- Needs a kept earlier release; retention never removes the active, previous or last known-good release.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/rollback-and-recovery](cloud/rollback-and-recovery.md)
