# pso docker:build

> Build the images with docker compose (requires a running Docker daemon)

Build the images with docker compose (requires a running Docker daemon).

## Usage

```text
pso docker:build [--service name] [--json]
```

## Flags

`--service`, `--json`.

## Examples

```bash
pso docker:build
```

## Failure cases

- `the Docker daemon is not running`: nothing is built. Start Docker Desktop and retry.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [devops/docker](devops/docker.md)
