# pso dev

> Validate, start, monitor and gracefully stop the workspace (Ctrl+C stops). Local processes by default; --docker runs the environment's containers instead. --service starts one service (and with --with-dependencies what it depends on: other services, its database and the identity provider), --fei also starts that frontend's development servers

Validate, start, monitor and gracefully stop the workspace (Ctrl+C stops). Local processes by default; --docker runs the environment's containers instead. --service starts one service (and with --with-dependencies what it depends on: other services, its database and the identity provider), --fei also starts that frontend's development servers.

## Usage

```text
pso dev [--only a,b] [--service <name>] [--with-dependencies] [--fei <name>] [--docker] [--env <name>] [--install] [--timeout 120] [--smoke]
```

## Flags

| Flag | What it does |
|---|---|
| `--only` | Start only the comma-separated service names. |
| `--service` | See the usage line and the task guide for this option. |
| `--with-dependencies` | See the usage line and the task guide for this option. |
| `--fei` | See the usage line and the task guide for this option. |
| `--docker` | See the usage line and the task guide for this option. |
| `--env` | See the usage line and the task guide for this option. |
| `--install` | Install missing Node dependencies before starting. |
| `--timeout` | Set the readiness wait in seconds. |
| `--smoke` | Start, check readiness and stop for a bounded smoke run. |

## Examples

```bash
pso dev
pso dev --only gateway --install
pso dev --smoke --timeout 60
pso dev --service catalog --with-dependencies
pso dev --docker --env local
pso dev --service gateway --with-dependencies --fei shop
```

## What it changes

- Validates the workspace, starts selected services in dependency order, waits for health and writes logs under `.pso/logs/`.
- On Ctrl+C it stops the processes it started; `--smoke` starts, checks and stops automatically.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
Starting 2 service(s)...
[gateway] {"level":"info","msg":"orchestrator listening",...}

Ready:
  catalog   http://127.0.0.1:8081   READY/UP
  gateway   http://127.0.0.1:3000   READY/UP
```

## Verify the result

- Run `pso services:health` in another terminal and read `.pso/logs/<service>.log` if a service is not ready.
- Send one authenticated HTTP request; health alone does not exercise route policy or the handler.

## Failure cases

- `port N is already in use`: another process owns it.
- `dependencies are not installed`: run `npm install` or pass `--install`.
- A service that crashes or is not ready in time aborts startup and rolls back the services already started.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [devops/development-environment](devops/development-environment.md)
