# pso docker:up

> Start the environment's containers in dependency order and wait until every one is healthy (development: missing infrastructure is provisioned and missing images are built first). --service starts those containers and what they depend on

Start the environment's containers in dependency order and wait until every one is healthy (development: missing infrastructure is provisioned and missing images are built first). --service starts those containers and what they depend on.

## Usage

```text
pso docker:up [--env <name>] [--service <a,b>] [--no-deps] [--build] [--vendor] [--timeout 300] [--json]
```

## Flags

`--env`, `--service`, `--no-deps`, `--build`, `--vendor`, `--timeout`, `--json`.

## Examples

```bash
pso docker:up
pso docker:up --env local --service catalog
pso docker:up --service catalog --no-deps --build
pso docker:up --timeout 600
```

## What it changes

- Development: provisions missing infrastructure and builds missing images first. Checks every published port, starts in dependency order and waits until every container is healthy.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
starting 8 container(s) of "local" in dependency order: idp -> accounts -> catalog-db -> catalog -> fei-shop -> fei-admin -> gateway -> edge
...
ready: https://shop.pso.localhost:8443/  https://admin.pso.localhost:8443/  api https://api.pso.localhost:8443
```

## Failure cases

- `host port(s) already in use`: names the port and the container; nothing was started.
- Staging and production with pending infrastructure changes: apply them first with `--yes`.
- A container that does not become healthy: exit 1 and `pso docker:logs <name>`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [infrastructure/containers](infrastructure/containers.md)
