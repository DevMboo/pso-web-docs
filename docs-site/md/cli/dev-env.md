# pso dev:env

> Show where every variable of a service comes from when `pso dev` starts it (its files, the environment of pso dev, the workspace, the infrastructure environment) and which infrastructure settings are not applied and why. Names and sources only: no value is ever shown, secrets are only reported as resolved

Show where every variable of a service comes from when `pso dev` starts it (its files, the environment of pso dev, the workspace, the infrastructure environment) and which infrastructure settings are not applied and why. Names and sources only: no value is ever shown, secrets are only reported as resolved.

## Usage

```text
pso dev:env [--service <name>] [--env <name>] [--json]
```

## Flags

| Flag | What it does |
|---|---|
| `--service` | Show one service only. |
| `--env` | Use this infrastructure environment instead of the contract's default. |
| `--json` | See the usage line and the task guide for this option. |

## Examples

```bash
pso dev:env
pso dev:env --service gateway
pso dev:env --service gateway --env local --json
```

## What it changes

- Assembles the environment of each service exactly as `pso dev` would (its files, the environment of pso dev, the workspace declaration, `.infra/env/<env>/<service>.env`) without starting anything, and prints every variable name with its source.
- Lists the infrastructure settings that are not applied and why: the identity settings are skipped as one group when the application selects its own key source, and a variable the service's files or the environment already set is never replaced.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
gateway   (infrastructure environment "local": .infra/env/local/gateway.env)
  PSO_JWT_PUBLIC_KEY_FILE            the environment of pso dev
  REDIS_URL                          .infra/env/local/gateway.env   [secret, resolved at start; value not shown]
  not applied from .infra/env/local/gateway.env: PSO_JWT_AUDIENCE, PSO_JWT_ISSUER, PSO_JWT_JWKS_URI, ...
    because the application selects its own key source (PSO_JWT_PUBLIC_KEY_FILE): the identity settings of the infrastructure are not applied
```

## Verify the result

- A `[secret ...]` mark means the value was resolved from the environment's secret provider; no value is ever printed.
- `not applied ... selects its own key source` explains why a service authenticates with its own keys in process mode.

## Failure cases

- Exit code 1 when a secret the service needs is not provisioned (`pso infra:apply`).

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [devops/development-environment](devops/development-environment.md)
- [infrastructure/contract](infrastructure/contract.md)
