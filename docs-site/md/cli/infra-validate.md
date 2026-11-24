# pso infra:validate

> Validate an environment's infrastructure: integrity of authorities, certificates and keys, expiry, permissions, drift, ignore rules, secrets leaked into files, and with --probe the running environment (TLS, security headers, API readiness, identity provider) for real (exit 1 on errors)

Validate an environment's infrastructure: integrity of authorities, certificates and keys, expiry, permissions, drift, ignore rules, secrets leaked into files, and with --probe the running environment (TLS, security headers, API readiness, identity provider) for real (exit 1 on errors).

## Usage

```text
pso infra:validate [--env <name>] [--probe] [--json]
```

## Flags

`--env`, `--probe`, `--json`.

## Examples

```bash
pso infra:validate
pso infra:validate --env local --probe
pso infra:validate --env staging --json
```

## What it changes

- Reads only. With `--probe` sends real HTTPS requests through the edge with certificate verification.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
ok      probe  api: readiness through the edge          HTTP 200, TLS verified (TLSv1.3) (8 ms)
ok      probe  identity: discovery                      HTTP 200, TLS verified (TLSv1.3) (27 ms)
OK: environment local
```

## Failure cases

- Exit code 1 on any error: missing or invalid resources, exposed keys, a secret value inside a generated file or `.env`, `.infra/` not ignored by git, a failed probe.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [infrastructure/security-and-certificates](infrastructure/security-and-certificates.md)
