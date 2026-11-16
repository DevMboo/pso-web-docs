# pso setup

> Install a signed .psopkg into an installation root (verified completely before anything is written; side by side with other versions)

Install a signed .psopkg into an installation root (verified completely before anything is written; side by side with other versions).

## Usage

```text
pso setup --package <file.psopkg> --root <dir> --trust <public.pem[,more]> [--key <recipient-private.pem>] [--no-activate] [--json]
```

## Flags

`--package`, `--root`, `--trust`, `--key`, `--no-activate`, `--json`.

## Examples

```bash
pso setup --package pso.psopkg --root C:Toolspso --trust release.public.pem
```

## Failure cases

- The package is verified completely before anything is written. A package signed by an untrusted key, a root that is a system or home directory, or an existing version is refused.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
