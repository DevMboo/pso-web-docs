# pso frontend:declare

> Write deploy/<name>.deployment.pso.ts for a frontend project so the native deployment engine can plan, deploy, verify and roll it back (`pso release:plan`, `release:deploy`, `release:rollback`)

Write deploy/<name>.deployment.pso.ts for a frontend project so the native deployment engine can plan, deploy, verify and roll it back (`pso release:plan`, `release:deploy`, `release:rollback`).

## Usage

```text
pso frontend:declare <project> --connection <name> [--environment production] [--site <name>] [--base-path /commerce/] [--api-port 3000 | --attach-to <application>] [--orchestrator-path /api] [--channels-path /channels] [--auth none|dev-token|oidc] [--oidc-issuer <url> --oidc-client <id> --oidc-authorize <url> --oidc-token <url> [--oidc-logout <url>]] [--require-signature] [--trust-key <keyId>=<public key>] [--probe-port 8080] [--domain shop.example.com] [--route /catalog/sku/:id,/files/*] [--connect-src https://idp.example.com] [--retain 5] [--dry-run] [--force]
```

## Flags

`--connection`, `--environment`, `--site`, `--base-path`, `--api-port`, `--attach-to`, `--orchestrator-path`, `--channels-path`, `--auth`, `--oidc-issuer`, `--oidc-client`, `--oidc-authorize`, `--oidc-token`, `--oidc-logout`, `--require-signature`, `--trust-key`, `--probe-port`, `--domain`, `--route`, `--connect-src`, `--retain`, `--dry-run`, `--force`.

## Examples

```bash
pso frontend:declare fei-commerce --connection azure --base-path /commerce/ --api-port 3000 --probe-port 8080
pso frontend:declare customers --connection azure --environment production --probe-port 8080 --retain 5
```

## What it changes

- Writes `deploy/<workspace>-fei.deployment.pso.ts` (shell) or `deploy/<workspace>-<domain>.deployment.pso.ts` (microfrontend) for the native engine: a static service, strategy `static`, the build output as an artifact. It holds no credential, only the name of a connection.
- Then use the normal commands: `pso release:plan`, `pso release:approve`, `pso release:deploy`, `pso release:rollback`.

## Failure cases

- The shell needs `--api-port`, the loopback port that always reaches the orchestrator's active release (its `internalPort`).
- A microfrontend declaration never gets an nginx section: its releases are served through the shell's configuration.
- An existing declaration is not overwritten without `--force`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [frontend/native-deployment](frontend/native-deployment.md)
