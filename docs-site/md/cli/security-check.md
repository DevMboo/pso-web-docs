# pso security:check

> Examine real configuration and sources for security problems, for one environment and with the configuration sources the application runs with in it (exit 1 on high/critical; --strict also medium)

Examine real configuration and sources for security problems, for one environment and with the configuration sources the application runs with in it (exit 1 on high/critical; --strict also medium).

## Usage

```text
pso security:check [--env development|test|staging|production] [--strict] [--json]
```

## Flags

| Flag | What it does |
|---|---|
| `--env` | The environment to evaluate: development, test, staging or production. |
| `--strict` | Treat medium findings as failures as well as high and critical findings. |
| `--json` | Print findings for tools without exposing secret values. |

## Examples

```bash
pso security:check
pso security:check --env production --strict
pso security:check --json
```

## What it changes

- Reads route policies, keys, Spring configuration and the security configuration of ONE environment (`--env`, else PSO_ENV, else development) from the sources the application itself reads in it: for development that includes the project's `.env` files, for production only the process environment.
- The output names the environment and where each project's configuration came from. It does not issue a JWT or send an HTTP request.

## Verify the result

- Use the reported resource and finding id to correct the declaration.
- A clean development result says nothing about production: run `pso security:check --env production` where the production variables are set (a pipeline, the platform).
- After a clean report, test a protected route with no token, a token missing scope and a correctly scoped token.

## Failure cases

- Exit code 1 on high or critical findings (`--strict` also medium). Findings never include secrets.
- `SEC005 ... is not configured for "development"`: put the `PSO_JWT_*` variables in the orchestrator's `.env`, or declare `security.authentication.*` in `pso.config.json`.
- `SEC013`: the development identity provider (`pso idp:dev`) is configured for production.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [security/auditing](security/auditing.md)
- [devops/environment-configuration](devops/environment-configuration.md)
