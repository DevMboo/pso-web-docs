# pso update

> Check for, install (side by side, verified, atomically activated) and roll back signed updates; offline packages are supported

Check for, install (side by side, verified, atomically activated) and roll back signed updates; offline packages are supported.

## Usage

```text
pso update check --catalog <file|https url> [--channel <stable|beta|dev>] [--root <dir>] [--trust <public.pem>] | install [--catalog <file|https url> | --package <file.psopkg>] [--channel <stable|beta|dev>] [--key <recipient-private.pem>] [--allow-downgrade] [--root <dir>] [--trust <public.pem>] | rollback [--root <dir>] [--json]
```

## Flags

`--catalog`, `--channel`, `--root`, `--trust`, `--package`, `--key`, `--allow-downgrade`, `--json`.

## Examples

```bash
pso update check --catalog catalog.json --root C:Toolspso
pso update install --catalog catalog.json --root C:Toolspso
pso update install --package pso-1.1.0.psopkg --root C:Toolspso
pso update rollback --root C:Toolspso
```

## Failure cases

- Catalogs must be signed, unexpired and newer than the last one seen (replay protection). Package size and SHA-256 are checked, the new version is verified before activation, and the previous version is kept for rollback.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
