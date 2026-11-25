# pso create

> Create a workspace, an orchestrator, a Spring Boot service, or a frontend project: the application shell (fei) or a microfrontend (mei)

Create a workspace, an orchestrator, a Spring Boot service, or a frontend project: the application shell (fei) or a microfrontend (mei).

## Usage

```text
pso create workspace <name> [--layout standard|flat] | orchestrator <name> [--crud-service <service>] | service <name> --runtime spring-boot [--package com.example.name] [--db postgres|none] | fei <name> | mei <name> [--host <fei>] [--register [--public | --scopes a,b]] [--install] [--template crud|empty] [--structure legacy] [--port <1024-65535>] [--prefix /path] [--dry-run] [--force] [--json]
```

## Flags

| Flag | What it does |
|---|---|
| `--layout` | `create workspace` only: `standard` (default) or `flat` (the 0.1 layout, kept for compatibility). |
| `--crud-service` | The service the orchestrator's products module calls (default: the service that serves /products). |
| `--runtime` | See the usage line and the task guide for this option. |
| `--package` | The Java base package of a service (default `com.pso.<name>`). |
| `--db` | Generate PostgreSQL support for a Spring service (the reference CRUD always uses it; `none` only with `--template empty`). |
| `--host` | See the usage line and the task guide for this option. |
| `--register` | See the usage line and the task guide for this option. |
| `--public` | See the usage line and the task guide for this option. |
| `--scopes` | See the usage line and the task guide for this option. |
| `--install` | See the usage line and the task guide for this option. |
| `--template` | `crud` (default): the reference Product CRUD; `empty`: the same foundations without it. |
| `--structure` | `legacy` creates the 0.1 layout instead of the official modular / layered one. |
| `--port` | Assign the service port instead of the next generated default. |
| `--prefix` | Set the Spring service's URL prefix. |
| `--dry-run` | Preview the files and registration without writing them. |
| `--force` | Allow replacement of conflicting generated files after reviewing them. |
| `--json` | Print machine-readable command output. |

## Examples

```bash
pso create workspace shop
cd shop
pso create orchestrator gateway
pso create service catalog
pso create service orders --db postgres
pso create fei shop --install
pso create mei customers --register --install
```

## What it changes

- `workspace` creates `shop/` with the standard layout: `.infra/` (only its ignore rule; nothing is provisioned), `orchestrators/`, `services/`, `frontends/`, `frontends/modules/`, `shared/`, `docs/`, `tests/`, `pso.workspace.json` and `README.md`; it does not start a process. `--layout flat` keeps the 0.1 layout.
- `orchestrator` goes to `orchestrators/<name>`, `service` to `services/<name>`, `fei` to `frontends/fei-<name>` and `mei` to `frontends/modules/mei-<name>`; each path is registered with the name, port and role in the workspace file. In a flat 0.1 workspace new projects follow the existing ones (root and `frontend/`).
- `--db postgres` adds the Spring data dependency, datasource configuration and a Flyway migration template.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
created    orchestrators/gateway/src/main.ts
created    orchestrators/gateway/package.json
...
registered "gateway" in pso.workspace.json (port 3000)
```

## Verify the result

- Open `pso.workspace.json` to confirm the declared services and ports.
- Run `pso validate` for structural checks, then `pso services:list` to see the registry. Neither command proves that a service is running.

## Failure cases

- `A workspace has exactly one public entry point`: a second orchestrator is refused.
- `Invalid service name`: names are kebab-case; `../x` and absolute paths are rejected before anything is written.
- Existing files with different content are reported as conflicts and left untouched unless you pass `--force`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [getting-started/first-workspace](getting-started/first-workspace.md)
- [getting-started/first-orchestrator](getting-started/first-orchestrator.md)
- [getting-started/first-microservice](getting-started/first-microservice.md)
