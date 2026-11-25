# pso frontend:config

> Configure how users sign in to a FEI. With --env: record the sign-in mode of that FEI in that environment of pso.infra.json and re-render its site configuration through the infrastructure engine (other FEIs and environments unchanged). With --site: write the FEI's runtime configuration on a site root you manage: where each approved MEI's release pointer is, who may sign releases, how users sign in and the orchestrator's same-origin path. It holds no secret and is never built into the bundle

Configure how users sign in to a FEI. With --env: record the sign-in mode of that FEI in that environment of pso.infra.json and re-render its site configuration through the infrastructure engine (other FEIs and environments unchanged). With --site: write the FEI's runtime configuration on a site root you manage: where each approved MEI's release pointer is, who may sign releases, how users sign in and the orchestrator's same-origin path. It holds no secret and is never built into the bundle.

## Usage

```text
pso frontend:config --env <name> [--fei <name>] --auth oidc|dev-token|none [--json] | --site <dir> [--fei <name>] [--auth dev-token|oidc|none] [--oidc-issuer <url>] [--oidc-authorize <url>] [--oidc-token <url>] [--oidc-client <id>] [--oidc-scope "openid"] [--api-base /api] [--channels-path /channels] [--require-signature] [--trust-signer <name>] [--trust-key <public-or-private-pem>] [--key-id <id>]
```

## Flags

`--env`, `--fei`, `--auth`, `--json`, `--site`, `--oidc-issuer`, `--oidc-authorize`, `--oidc-token`, `--oidc-client`, `--oidc-scope`, `--api-base`, `--channels-path`, `--require-signature`, `--trust-signer`, `--trust-key`, `--key-id`.

## Examples

```bash
pso frontend:config --env local --fei shop --auth none
pso frontend:config --env local --fei admin --auth oidc
pso frontend:config --site ./site --auth none
pso frontend:config --site ./site --auth oidc --oidc-issuer https://idp.example --oidc-authorize https://idp.example/authorize --oidc-token https://idp.example/token --oidc-client commerce-web --require-signature --trust-key release.pem --key-id release-1
```

## What it changes

- With `--env`: records the sign-in mode of that FEI in that environment (`environments.<env>.frontends.<fei>.auth` in `pso.infra.json`) and lets the infrastructure engine re-render its `pso-runtime-config.json`; later provisioning and publication keep it, and other FEIs and environments are unchanged.
- With `--site`: writes `pso-runtime-config.json` on a site root you manage: where each approved MEI's pointer is, the sign-in provider, the same-origin API path and the trusted signing keys. It holds no secret and is not built into the bundle.

## Failure cases

- `--env` with several FEIs needs `--fei`; `--auth` is required with `--env`; the OIDC, API-path and signature options are refused with `--env` (they come from `pso.infra.json`).
- `dev-token` is refused in a production environment.
- `--require-signature needs at least one --trust-key`.
- OpenID Connect endpoints must be `https://` URLs.
- `dev-token` works only in a shell built with `--allow-dev-token`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [frontend/releases](frontend/releases.md)
- [frontend/authentication-and-api](frontend/authentication-and-api.md)
