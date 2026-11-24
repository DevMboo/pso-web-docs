# pso deploy

> Apply a deployment (requires --yes); verifies checksums and health, rolls back on failed verification. --env deploys an environment of pso.infra.json (Docker target: build, record, start, verify through the edge, automatic rollback; bundle target: a reviewable bundle for the operator); --dry-run only plans

Apply a deployment (requires --yes); verifies checksums and health, rolls back on failed verification. --env deploys an environment of pso.infra.json (Docker target: build, record, start, verify through the edge, automatic rollback; bundle target: a reviewable bundle for the operator); --dry-run only plans.

## Usage

```text
pso deploy --target native --dir <path> [--release 1.0.0] [--health-url url ...] [--health-timeout 30] [--allow-snapshot] --yes [--json] | --target compose --yes [--json] | --env <name> [--dry-run] [--timeout 300] [--yes] [--json]
```

## Flags

`--target`, `--dir`, `--release`, `--health-url`, `--health-timeout`, `--allow-snapshot`, `--yes`, `--json`, `--env`, `--dry-run`, `--timeout`.

## Examples

```bash
pso deploy --target native --dir D:\apps\shop --release 1.0.0 --yes --health-url http://127.0.0.1:3000/health/ready
```

## Failure cases

- Exit code 3 without `--yes`: deployment needs explicit authorization.
- A failing `--health-url` rolls back automatically.
- `Refusing to deploy a SNAPSHOT build`: the service's own `<version>` or its `<pso.version>` is a SNAPSHOT. A release uses fixed versions; a development deployment says so with `--allow-snapshot`, which is never accepted with PSO_ENV=production.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [devops/build-and-deploy](devops/build-and-deploy.md)
