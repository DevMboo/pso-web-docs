# pso infra:token

> Get an access token for a development user: signed offline (pso-dev identity) or issued by the environment's Keycloak (password grant through the development-only API client). Development environments only

Get an access token for a development user: signed offline (pso-dev identity) or issued by the environment's Keycloak (password grant through the development-only API client). Development environments only.

## Usage

```text
pso infra:token [--env <name>] --user <name> [--scopes a,b] [--ttl 900] [--json]
```

## Flags

`--env`, `--user`, `--scopes`, `--ttl`, `--json`.

## Examples

```bash
pso infra:token --user ana
pso infra:token --user ana --scopes catalog:read --ttl 600
pso infra:token --env local --user bob --json
```

## What it changes

- pso-dev identity: signs a development token offline. Keycloak: obtains a real token for a declared development user through the development-only API client.

## Failure cases

- Development environments only.
- `--scopes` applies to the pso-dev identity; Keycloak issues the scopes the user has.
- `the identity provider refused the token request`: start the stack (pso docker:up) or restart idp after a realm change.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
