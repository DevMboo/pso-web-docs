# pso docker:build

> Build the service images. With pso.infra.json: for the environment's generated topology (Dockerfiles are created when missing, never overwritten; --vendor copies the framework packages into the services). Without it: the 0.1 behaviour (docker-compose.yml from pso docker:init)

Build the service images. With pso.infra.json: for the environment's generated topology (Dockerfiles are created when missing, never overwritten; --vendor copies the framework packages into the services). Without it: the 0.1 behaviour (docker-compose.yml from pso docker:init).

## Usage

```text
pso docker:build [--env <name>] [--service <name>] [--vendor] [--json]
```

## Flags

`--env`, `--service`, `--vendor`, `--json`.

## Examples

```bash
pso docker:build
pso docker:build --env local --vendor
pso docker:build --service catalog
```

## What it changes

- With pso.infra.json: creates a Dockerfile only where none exists, vendors the framework packages with `--vendor`, builds the images of the environment's topology. Without it: the 0.1 behaviour with `docker-compose.yml`.

## Failure cases

- `resolve to a path on this machine`: re-run with `--vendor`.
- `--env and --vendor need pso.infra.json` in a 0.1 workspace that still uses docker-compose.yml.
- Docker not running: nothing is built.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [infrastructure/containers](infrastructure/containers.md)
