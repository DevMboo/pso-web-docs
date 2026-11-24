# pso infra:identity

> Reconcile the environment's running Keycloak realm with the declarations through its administration API: FEI clients, redirect URIs, web origins, the service client, scope roles, mappers and development users. Without --apply it reports drift; changing an existing managed client or removing one (--prune) needs --yes. Users, credentials, sessions, client secrets and resources PSO did not create are never changed

Reconcile the environment's running Keycloak realm with the declarations through its administration API: FEI clients, redirect URIs, web origins, the service client, scope roles, mappers and development users. Without --apply it reports drift; changing an existing managed client or removing one (--prune) needs --yes. Users, credentials, sessions, client secrets and resources PSO did not create are never changed.

## Usage

```text
pso infra:identity [--env <name>] [--apply] [--prune] [--yes] [--json]
```

## Flags

`--env`, `--apply`, `--prune`, `--yes`, `--json`.

## Examples

```bash
pso infra:identity
pso infra:identity --apply
pso infra:identity --apply --yes
pso infra:identity --env local --apply --prune --yes --json
```

## What it changes

- Compares the running identity provider (Keycloak administration API) with the declarations: one OIDC client per FEI with its redirect URIs and web origins, the development API client, one role per scope, development users.
- Only objects marked `pso.managed` are changed. Users, credentials, sessions, client secrets and the realm itself are never changed or recreated.
- `--apply` makes the routine changes (create what is missing); changing an existing managed client needs `--yes`; removing one also needs `--prune`.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
realm pso-reference (reachable): in sync with the declarations

Nothing was changed. Apply with --apply (sensitive changes also need --yes).
```

## Failure cases

- Exit 3: a sensitive change was found without `--yes`; nothing was changed.
- `the identity provider refused the administrator sign-in`: the `idp-admin` secret is not the one Keycloak runs with.
- `external-conflict`: a client with the FEI's clientId exists without the marker; it is never modified.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [infrastructure/identity-reconciliation](infrastructure/identity-reconciliation.md)
