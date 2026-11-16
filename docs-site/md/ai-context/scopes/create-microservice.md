# Task: create a microservice

> Exact steps to add a Spring Boot service, optionally with PostgreSQL.

1. In the workspace root: `pso create service <name> --runtime spring-boot` (kebab-case; add `--db postgres` only if the service owns a database).
2. Install the PSO Java artifacts once with `pso java:install` (the create command prints a note when they are missing). Check the service: `pso validate`, `pso security:validate`, then `.\mvnw.cmd test` inside the service folder (`./mvnw test` on macOS/Linux). The generated tests must pass. The endpoint prefix is derived from the name (`--prefix /x` overrides it).
3. The service is deny-by-default (security starter) and returns `401` for everything except `/actuator/health`. That is correct; do not add `permitAll()`. A database (`pso database:enable`) and dispatch are off until chosen.
4. Set `PSO_JWT_JWKS_URI`, `PSO_JWT_ISSUER`, `PSO_JWT_AUDIENCE` for the service (ask the user for the values; never invent them).
5. Add behavior with `pso make:resource <name> --service <svc> --route /path --scopes a:read` (creates the orchestrator module and the Spring controller together), and generators such as `pso make:service`, `pso make:dto`, `pso make:repository` with `--service <svc>`.
6. With PostgreSQL: `pso make:migration <name> --service <svc>`; never edit an applied migration.
7. Declare dependencies in `pso.workspace.json` (`dependsOn`) or with `pso services:register`.
