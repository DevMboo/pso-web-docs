# pso tls:check

> Verify the workspace's TLS material: files, issuing authority, validity dates, private keys and the host names the orchestrator will verify (exit 1 on any problem; no key material is printed)

Verify the workspace's TLS material: files, issuing authority, validity dates, private keys and the host names the orchestrator will verify (exit 1 on any problem; no key material is printed).

## Usage

```text
pso tls:check [--dir tls] [--hosts name,name] [--json]
```

## Flags

`--dir`, `--hosts`, `--json`.

## Examples

```bash
pso tls:check
pso tls:check --hosts idp --json
```

## Failure cases

- Exit 1 lists each problem: an expired certificate, a certificate that does not name its service, a key that does not belong to it, another issuing authority, a missing file.
- `No TLS directory`: run `pso tls:init --dev` or place your certificates in ./tls.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [devops/docker](devops/docker.md)
- [security/tls-and-mtls](security/tls-and-mtls.md)
