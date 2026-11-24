# pso deploy:plan

> Show exactly what a deployment would change, without changing anything (--env: the 0.2 environment deployment of pso.infra.json)

Show exactly what a deployment would change, without changing anything (--env: the 0.2 environment deployment of pso.infra.json).

## Usage

```text
pso deploy:plan --target native --dir <path> [--release 1.0.0] [--health-url url] [--allow-snapshot] [--json] | --target compose [--json] | --env <name> [--json]
```

## Flags

`--target`, `--dir`, `--release`, `--health-url`, `--allow-snapshot`, `--json`, `--env`.

## Examples

```bash
pso deploy:plan --target native --dir D:\apps\shop --release 1.0.0
pso deploy:plan --target native --dir D:\apps\shop --allow-snapshot   # a development deployment
```

## Failure cases

- `does not match its recorded checksum`: an artifact was modified after `pso build`.
- `Refusing to deploy a SNAPSHOT build`: names the version at fault (the service's own, or `<pso.version>`).

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [devops/build-and-deploy](devops/build-and-deploy.md)
