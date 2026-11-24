# pso make:crud

> Generate a complete CRUD resource in the official structures: in a Spring service (entity, Flyway migration, repository with database-level filters, request/response records, mapper, transactional service, controller with scopes, tests) and/or, with --orchestrator, the matching orchestrator module (DTOs, contract, provider, service, mapper, controller, routes) bound to that service

Generate a complete CRUD resource in the official structures: in a Spring service (entity, Flyway migration, repository with database-level filters, request/response records, mapper, transactional service, controller with scopes, tests) and/or, with --orchestrator, the matching orchestrator module (DTOs, contract, provider, service, mapper, controller, routes) bound to that service.

## Usage

```text
pso make:crud <resource> [--service <spring-service>] [--orchestrator] [--to <service>] [--plural <name>] [--fields "code:string:unique:filter:sort,total:decimal:precision=12:scale=2:min=0"] [--module <name>] [--dry-run] [--json]
```

## Flags

`--service`, `--orchestrator`, `--to`, `--plural`, `--fields`, `--module`, `--dry-run`, `--json`.

## Examples

```bash
pso make:crud order --service sales --orchestrator --dry-run
pso make:crud order --service sales --orchestrator --fields "code:string:unique:length=3-32:filter:sort,total:decimal:precision=12:scale=2:min=0:sort,paid:boolean:default=false:filter"
pso make:crud supplier --service catalog
```

## What it changes

- In the Spring service: entity, the next Flyway migration, repository and filters, request/response records, mapper, transactional service, controller with `<plural>:read` / `<plural>:write`, tests.
- With `--orchestrator`: the `<plural>` module bound to the service (DTOs, contract, provider, service, mapper, controller, routes with a public `/options`), a test and a Studio request collection.

## Verify the result

- `pso validate`, then `./mvnw test` in the service and `npm test` in the orchestrator.

## Failure cases

- `already has the <name> resource`: existing code is never overwritten; choose another name.
- `has no database`: enable PostgreSQL first (`pso database:enable postgresql --service <svc>`).
- `follows the 0.1 structure`: make:crud generates the official structure; use `pso make:resource` there or migrate (`pso structure:inspect`).
- `Module "<m>" already exists`: pass another `--module`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [architecture/generators](architecture/generators.md)
- [architecture/reference-crud](architecture/reference-crud.md)
