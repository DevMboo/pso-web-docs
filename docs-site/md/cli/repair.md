# pso repair

> Restore damaged or missing files of the installed version from an authenticated package (never upgrades; user data is never touched)

Restore damaged or missing files of the installed version from an authenticated package (never upgrades; user data is never touched).

## Usage

```text
pso repair [--quick | --full] [--dry-run] [--package <file.psopkg>] [--key <recipient-private.pem>] [--root <dir>] [--trust <public.pem>] [--json]
```

## Flags

`--quick`, `--full`, `--dry-run`, `--package`, `--key`, `--root`, `--trust`, `--json`.

## Examples

```bash
pso repair --dry-run --root C:Toolspso
pso repair --full --root C:Toolspso
pso repair --package pso.psopkg --root C:Toolspso
```

## Failure cases

- Repair restores files of the installed version only from an authenticated package of that same version and never upgrades or touches configuration, cache or logs. If a replacement fails, earlier replacements are rolled back.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
