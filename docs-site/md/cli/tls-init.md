# pso tls:init

> Create DEVELOPMENT TLS certificates for the workspace's services (a local certificate authority and one certificate per service, named after the service) so a local stack runs with verified TLS. Production certificates are supplied in the same layout, never generated here

Create DEVELOPMENT TLS certificates for the workspace's services (a local certificate authority and one certificate per service, named after the service) so a local stack runs with verified TLS. Production certificates are supplied in the same layout, never generated here.

## Usage

```text
pso tls:init --dev [--days 365] [--hosts name,name] [--dir tls] [--force] [--json]
```

## Flags

`--dev`, `--days`, `--hosts`, `--dir`, `--force`, `--json`.

## Examples

```bash
pso tls:init --dev
pso tls:init --dev --hosts idp --days 90
```

## Failure cases

- Without `--dev` nothing is created: the command only makes development certificates.
- `ca.crt exists without its key`: the directory holds certificates that were not created here; use `--dir` for another directory or `--force` to replace them.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [devops/docker](devops/docker.md)
- [security/tls-and-mtls](security/tls-and-mtls.md)
