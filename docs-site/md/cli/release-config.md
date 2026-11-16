# pso release:config

> The effective configuration of the ACTIVE release as the server holds it: services, ports, routes, health, environment variables (values that look like secrets are redacted) and secret reference names. Never a secret value.

The effective configuration of the ACTIVE release as the server holds it: services, ports, routes, health, environment variables (values that look like secrets are redacted) and secret reference names. Never a secret value..

## Usage

```text
pso release:config <application> [--json]
```

## Flags

`--json`.

## Examples

```bash
pso release:config shop-gateway
pso release:config shop-gateway --json
```

## What it changes

- Shows the effective configuration of the active release as the server holds it: services, routes, ports, health, instances, and environment variables with values that look like secrets redacted. Secrets appear only as a count of references, never as values.

## Failure cases

- `release: none` means no release is active yet.
- It shows the declaration deployed on the server, not the environment of the running process.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
