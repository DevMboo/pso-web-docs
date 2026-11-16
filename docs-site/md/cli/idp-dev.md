# pso idp:dev

> DEVELOPMENT identity provider for a workspace: init generates a signing key on this machine, its public key set, a JWKS server and a Compose overlay; token signs a development JWT; serve publishes the keys on loopback for `pso dev`. Never for production (refused with PSO_ENV=production)

DEVELOPMENT identity provider for a workspace: init generates a signing key on this machine, its public key set, a JWKS server and a Compose overlay; token signs a development JWT; serve publishes the keys on loopback for `pso dev`. Never for production (refused with PSO_ENV=production).

## Usage

```text
pso idp:dev init [--issuer https://idp:8443] [--audience pso-api] [--host idp] [--port 8443] [--dir idp] [--dry-run] [--force] [--json] | token [--sub dev-user] [--scopes a,b] [--roles x,y] [--ttl 900] [--audience pso-api] [--dir idp] [--json] | serve [--port 9443] [--dir idp] [--once]
```

## Flags

| Flag | What it does |
|---|---|
| `--issuer` | The `iss` of the tokens and the base URL of the JWKS endpoint (default `https://idp:8443`). |
| `--audience` | The `aud` the services require (default `pso-api`). |
| `--host` | The container name of the identity provider in the overlay (default `idp`). |
| `--port` | See the usage line and the task guide for this option. |
| `--dir` | See the usage line and the task guide for this option. |
| `--dry-run` | See the usage line and the task guide for this option. |
| `--force` | See the usage line and the task guide for this option. |
| `--json` | See the usage line and the task guide for this option. |
| `--sub` | The subject of the token (default `dev-user`). |
| `--scopes` | Scopes of the token, comma separated. |
| `--roles` | Roles of the token, comma separated. |
| `--ttl` | Lifetime of the token in seconds (default 900). |
| `--once` | See the usage line and the task guide for this option. |

## Examples

```bash
pso idp:dev init
pso tls:init --dev --hosts idp
docker compose -f docker-compose.yml -f docker-compose.idp.yml up -d --wait
pso idp:dev token --scopes orders:read,orders:write
pso idp:dev token --sub alice --roles admin --ttl 300 --json
pso idp:dev serve --port 9443
```

## What it changes

- `init` generates a signing key pair on this machine (`idp/keys/`, git-ignored), its public key set (`idp/jwks.json`), a dependency-free JWKS server (`idp/dev-idp.cjs`) and the Compose overlay `docker-compose.idp.yml`, and prints the `PSO_JWT_*` values for `.env`.
- `token` signs a development JWT (ES256) for a synthetic identity and prints only the token, so it can be used as `$(pso idp:dev token ...)`.
- `serve` publishes the public keys on loopback over plain HTTP for `pso dev`.
- The JWKS server holds public keys only: the private key never enters a container or an image.

## Verify the result

- `curl -H "Authorization: Bearer $(pso idp:dev token --scopes orders:read)" http://127.0.0.1:3000/orders` through the orchestrator.
- A request without the token answers 401; with a token missing the scope, 403.

## Failure cases

- `does not run with PSO_ENV=production`: it is a development tool; production uses a real identity provider.
- `No development identity provider`: run `pso idp:dev init` first.
- `The signing key ... is missing`: the key exists only on the machine where `init` ran.
- `pso security:check --env production` reports SEC013 when a production configuration names this issuer.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [security/authentication](security/authentication.md)
- [devops/docker](devops/docker.md)
- [tutorials/authentication](tutorials/authentication.md)
