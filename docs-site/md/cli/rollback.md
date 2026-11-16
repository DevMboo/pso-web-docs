# pso rollback

> Re-activate the previous release of a native deployment

Re-activate the previous release of a native deployment.

## Usage

```text
pso rollback --dir <path> --yes [--json]
```

## Flags

`--dir`, `--yes`, `--json`.

## Examples

```bash
pso rollback --dir D:\apps\shop --yes
```

## Failure cases

- Exit code 3 without `--yes`.
- `No previous release to roll back to` on a first release.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
