# pso storage:validate

> Validate the pso.storage.* configuration of the Spring services (provider values, secrets in files, limits that disagree, missing dependencies) before anything runs

Validate the pso.storage.* configuration of the Spring services (provider values, secrets in files, limits that disagree, missing dependencies) before anything runs.

## Usage

```text
pso storage:validate [--service <name>] [--json]
```

## Flags

`--service`, `--json`.

## Examples

```bash
pso storage:validate
pso storage:validate --service hr-service --json
```

## Failure cases

- Static checks only: unknown providers, missing starter or SDK, missing bucket or container, an account key in a staging or production file, signed URLs with a long lifetime, upload limits that disagree with the multipart limits. Credentials and connectivity are checked when the service starts. Exit 1 on a high or critical finding; secret values are never printed.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [spring/storage](spring/storage.md)
