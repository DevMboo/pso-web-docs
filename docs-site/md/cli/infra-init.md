# pso infra:init

> Write pso.infra.json, the declarative infrastructure contract (environments, hosts, TLS, secrets, identity provider), derived from the workspace: one local development environment to start with

Write pso.infra.json, the declarative infrastructure contract (environments, hosts, TLS, secrets, identity provider), derived from the workspace: one local development environment to start with.

## Usage

```text
pso infra:init [--identity keycloak|pso-dev|none] [--runtime docker|process] [--users ana,bob:catalog:read+cart:write] [--mtls accounts,billing] [--dry-run] [--force] [--json]
```

## Flags

`--identity`, `--runtime`, `--users`, `--mtls`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso infra:init
pso infra:init --identity keycloak --runtime docker --users ana,bob
pso infra:init --identity pso-dev --dry-run
```

## What it changes

- Writes `pso.infra.json` with one local development environment (domain `pso.localhost`) and the chosen identity provider. Nothing is provisioned yet.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
wrote pso.infra.json: environment "local" (development, pso.localhost, runtime docker), identity keycloak
next: pso infra:plan   then   pso infra:apply
```

## Failure cases

- `pso.infra.json already exists`: edit it, or pass `--force` to replace it.
- An unknown `--identity` or `--runtime` is refused before anything is written.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [infrastructure/contract](infrastructure/contract.md)
