# pso connection:key

> Diagnose the access rights of the connection's SSH private key file (OpenSSH refuses a key other users can read); with --repair --yes restrict the file to you after showing each command

Diagnose the access rights of the connection's SSH private key file (OpenSSH refuses a key other users can read); with --repair --yes restrict the file to you after showing each command.

## Usage

```text
pso connection:key <name> [--repair --yes] [--json]
```

## Flags

`--repair`, `--yes`, `--json`.

## Examples

```bash
pso connection:key azure
pso connection:key azure --repair --yes
```

## What it changes

- Reads the access list (Windows) or the mode (macOS, Linux) of the private key file the connection names, and says whether OpenSSH will accept it. It never reads the key's content. Without --repair nothing is changed. With --repair --yes it runs exactly the commands it printed, on that one file, and reports the rights it measured afterwards.

## Failure cases

- `UNPROTECTED PRIVATE KEY FILE` from OpenSSH is this problem: other accounts can read the key.
- A repair never runs silently: without --yes the command refuses, and Studio asks you to type the connection name.
- A key that is missing or unreadable is reported as such; the file is never created.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
