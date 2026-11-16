# pso frontend:config

> Write the FEI's runtime configuration on a site root: where each approved MEI's release pointer is, who may sign releases, how users sign in and the orchestrator's same-origin path. It holds no secret and is never built into the bundle

Write the FEI's runtime configuration on a site root: where each approved MEI's release pointer is, who may sign releases, how users sign in and the orchestrator's same-origin path. It holds no secret and is never built into the bundle.

## Usage

```text
pso frontend:config --site <dir> [--auth dev-token|oidc|none] [--oidc-issuer <url>] [--oidc-authorize <url>] [--oidc-token <url>] [--oidc-client <id>] [--oidc-scope "openid"] [--api-base /api] [--channels-path /channels] [--require-signature] [--trust-key <public-or-private-pem>] [--key-id <id>]
```

## Flags

`--site`, `--auth`, `--oidc-issuer`, `--oidc-authorize`, `--oidc-token`, `--oidc-client`, `--oidc-scope`, `--api-base`, `--channels-path`, `--require-signature`, `--trust-key`, `--key-id`.

## Examples

```bash
pso frontend:config --site ./site --auth none
pso frontend:config --site ./site --auth oidc --oidc-issuer https://idp.example --oidc-authorize https://idp.example/authorize --oidc-token https://idp.example/token --oidc-client commerce-web --require-signature --trust-key release.pem --key-id release-1
```

## What it changes

- Writes `pso-runtime-config.json` on the site root: where each approved MEI's pointer is, the sign-in provider, the same-origin API path and the trusted signing keys. It holds no secret and is not built into the bundle.

## Failure cases

- `--require-signature needs at least one --trust-key`.
- OpenID Connect endpoints must be `https://` URLs.
- `dev-token` works only in a shell built with `--allow-dev-token`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [frontend/releases](frontend/releases.md)
- [frontend/authentication-and-api](frontend/authentication-and-api.md)
