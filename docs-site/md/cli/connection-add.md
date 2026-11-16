# pso connection:add

> Register an SSH connection to a Linux server: pins its SSH host key after you approve the fingerprint, stores a key passphrase or password only in the protected credential store

Register an SSH connection to a Linux server: pins its SSH host key after you approve the fingerprint, stores a key passphrase or password only in the protected credential store.

## Usage

```text
pso connection:add <name> --host <host> --user <user> [--port 22] [--auth key|agent|password] [--key <private-key-file>] [--jump <user@host:port>] [--timeout 15] [--accept-host-key <SHA256:fingerprint>] [--passphrase-stdin] [--password-stdin] [--json]
```

## Flags

| Flag | What it does |
|---|---|
| `--host` | See the usage line and the task guide for this option. |
| `--user` | See the usage line and the task guide for this option. |
| `--port` | See the usage line and the task guide for this option. |
| `--auth` | See the usage line and the task guide for this option. |
| `--key` | See the usage line and the task guide for this option. |
| `--jump` | See the usage line and the task guide for this option. |
| `--timeout` | See the usage line and the task guide for this option. |
| `--accept-host-key` | The SHA256 fingerprint you verified out of band. |
| `--passphrase-stdin` | Read the key passphrase from standard input. |
| `--password-stdin` | Read the SSH password from standard input. |
| `--json` | See the usage line and the task guide for this option. |

## Examples

```bash
pso connection:add prod --host 203.0.113.10 --user deploy --auth key --key ~/.ssh/id_ed25519
pso connection:add prod --host 203.0.113.10 --user deploy --accept-host-key SHA256:AbCd...
pso connection:add prod --host 10.0.0.5 --user deploy --jump ops@bastion.example:22 --accept-host-key SHA256:AbCd...
```

## What it changes

- Asks the server for its SSH host keys and pins the one whose fingerprint you approve (known_hosts owned by PSO, `StrictHostKeyChecking=yes`).
- Stores a key passphrase or password only in the protected credential store; a private key file is referenced by path, never copied.

## Failure cases

- Exit code 3 in a non-interactive shell without `--accept-host-key`: the fingerprint must be approved by a person.
- `No protected credential store is available`: PSO never falls back to plain text; configure one (see cloud/credentials-and-security).
- Passwords and passphrases are never arguments: use `--password-stdin` / `--passphrase-stdin` or the hidden prompt.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/connect-a-server](cloud/connect-a-server.md)
