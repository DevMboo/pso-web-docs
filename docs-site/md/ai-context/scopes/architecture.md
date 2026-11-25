# Architecture rules

> Principles, runtimes, workspace, discovery, manifests and lifecycle.

## Principles

1. Convention over configuration, with explicit configuration when needed.
2. Static discovery: validation parses files, it does not execute them.
3. Deny by default for routes, guards and services.
4. Explicit versioned contracts between services; no shared domain entities.
5. Services are declared, never found by scanning; discovery is not authorization.
6. Deterministic output: sorted discovery, manifests with a sha256 digest and no timestamps.
7. Declared exposure: decorators never expose endpoints.

## Shape of a system

- One **Node.js + TypeScript orchestrator** (single public entry point, no domain logic) and any number of **Spring Boot microservices** (Java 21, own database, independent build).
- `pso.workspace.json` declares each service: `name`, `path`, `runtime` (`node` | `spring-boot`), `role` (`orchestrator` | `service`), `port`, `dependsOn`, `health`, optional `db`, optional `dev.command`.
- The orchestrator forwards guarded requests to services with a verified bearer token; services validate it again.

## Application structure (official, 0.2)

- Orchestrator: `src/.app/{configs,contracts,filters,guards,middlewares,interceptors,decorators}` for application-wide components; `src/modules/<module>/` with `MODULE.pso.ts`, `routes/ROUTES.pso.ts`, `controllers/`, `services/`, `providers/`, `contracts/`, `dto/request|response/`, `mappers/`; `src/shared/`; `src/main.ts` (bootstrap only). `tsconfig.json` must include `"src/.app/**/*"` (TypeScript skips dot-directories).
- Spring service (layered): `application/{rest,service,dto/request,dto/response,mapper}`, `infrastructure/{entity,persistence,config,security,filter,client}`, `shared/{exception,validation,constant}`; Flyway migrations in `src/main/resources/db/migration`; Hibernate `ddl-auto: validate`, never `update`. No providers, modules or `.app` in Java.
- `.infra/` (workspace) is operational infrastructure PSO generates and reconciles: never put application code there, never edit it by hand. It exists from `pso create workspace` on (holding only its ignore rule); `pso infra:init` and `pso infra:apply` fill it.
- Workspace layout: orchestrators in `orchestrators/`, microservices in `services/`, FEIs in `frontends/`, MEIs in `frontends/modules/`, workspace-level shared material (the frontends' orchestrator contract) in `shared/`, workspace documentation in `docs/`, cross-project tests in `tests/`. The generators decide the path and register it in `pso.workspace.json`; every command reads that `path`.
- New projects include a reference Product CRUD (`--template empty` omits it); the orchestrator's CRUD module is bound to the service that serves its path (`PRODUCT_SERVICE` + `metadata.services`), rebind with `pso crud:bind`.
- 0.1 projects (`src/routes/<module>/`, `routes/`, `ports/`, `adapters/` in Java) keep working; never mix the two in one project.

## Discovery, manifest, lifecycle

- Files are classified by suffix and directory in byte-wise sorted order. Kinds are listed in the orchestrator and microservice conventions.
- `pso validate --write-manifest` writes `.pso/manifest.json`.
- Resource lifecycle: DISCOVERED, VALIDATED, REGISTERED, INITIALIZED, READY, STOPPING, STOPPED, plus FAILED. Start is dependency-first, stop is reverse, failures roll back.

## Configuration precedence (low to high)

framework defaults, workspace, service, environment file, environment variables (`PSO_CFG_a__b`), `--set`. Secrets are references, never literals.

Layers (low to high): defaults, workspace, service, environment-file, environment-variables, cli. Environments: development, test, staging, production.

- `server.host` (string, default "127.0.0.1")
- `server.port` (number)
- `server.maxBodyBytes` (number, default 1048576)
- `server.upstreamTimeoutMs` (number, default 5000)
- `http.limits.maxBodyBytes` (number)
- `http.limits.maxUpstreamResponseBytes` (number)
- `http.limits.maxHeaderBytes` (number)
- `http.limits.upstreamTimeoutMs` (number)
- `http.limits.upstreamResponseTimeoutMs` (number)
- `http.limits.headersTimeoutMs` (number)
- `http.limits.requestTimeoutMs` (number)
- `http.limits.keepAliveTimeoutMs` (number)
- `http.limits.maxConnections` (number)
- `http.limits.maxConcurrentRequests` (number)
- `http.limits.maxUploadBytes` (number)
- `http.limits.maxMultipartParts` (number)
- `http.limits.maxConcurrentUploads` (number)
- `http.readiness.optional` (list)
- `http.readiness.intervalMs` (number)
- `http.readiness.timeoutMs` (number)
- `http.shutdown.graceMs` (number)
- `http.shutdown.cleanupMs` (number)
- `http.rateLimit.perMinute` (number)
- `http.rateLimit.burst` (number)
- `http.rateLimit.provider` (string)
- `http.rateLimit.redisUrl` (string)
- `http.rateLimit.onFailure` (string)
- `http.rateLimit.perPrincipal` (boolean)
- `http.rateLimit.maxKeys` (number)
- `http.rateLimit.prefix` (string)
- `http.rateLimit.trustedProxies` (list)
- `http.rateLimit.routes` (json)
- `http.diagnostics.token` (string)
- `security.authentication.provider` (string, default "jwt")
- `security.authentication.customProvider` (string)
- `security.authentication.issuer` (string)
- `security.authentication.audience` (list)
- `security.authentication.algorithms` (list, default ["ES256","RS256"])
- `security.authentication.clockToleranceSeconds` (number, default 5)
- `security.authentication.maxTokenBytes` (number, default 8192)
- `security.authentication.tokenType` (string)
- `security.authentication.rolesClaim` (string, default "roles")
- `security.authentication.permissionsClaim` (string, default "permissions")
- `security.authentication.jwks.uri` (string)
- `security.authentication.jwks.timeoutMs` (number, default 3000)
- `security.authentication.jwks.cacheTtlSeconds` (number, default 600)
- `security.authentication.jwks.trustedHosts` (list)
- `security.authentication.jwks.connectTimeoutMs` (number, default 2000)
- `security.authentication.jwks.initDeadlineMs` (number, default 20000)
- `security.authentication.jwks.privateHosts` (list)
- `security.authentication.jwks.caFile` (string)
- `security.authentication.publicKey.file` (string)
- `security.authorization.defaultPolicy` (string, default "authenticated")
- `security.propagation.forward` (string, default "bearer")
- `security.jwt.issuer` (string)
- `security.jwt.audience` (string)
- `security.jwt.jwksUri` (string)
- `security.jwt.publicKeyFile` (string)
- `security.jwt.algorithms` (string[], default ["RS256","ES256"])
- `security.rateLimit.perMinute` (number, default 600)
- `logging.level` (string, default "info")
- `logging.format` (string)
- `logging.color` (string)
- `logging.errorDetails` (boolean, default false)
- `dispatch.enabled` (boolean, default false)
- `dispatch.provider` (string, default "none")
- `dispatch.maxMessageBytes` (number, default 1048576)
- `dispatch.consumer.concurrency` (number, default 1)
- `dispatch.consumer.batchSize` (number, default 10)
- `dispatch.retry.enabled` (boolean, default true)
- `dispatch.retry.maxAttempts` (number, default 3)
- `dispatch.retry.initialDelay` (string, default "1s")
- `dispatch.retry.maxDelay` (string, default "30s")
- `dispatch.retry.multiplier` (number, default 2)
- `dispatch.deadLetter.enabled` (boolean, default true)
- `dispatch.observability.enabled` (boolean, default true)
- `dispatch.idempotency.retention` (string, default "24h")
- `dispatch.idempotency.lease` (string, default "30s")
- `dispatch.idempotency.store` (string, default "auto")
- `dispatch.idempotency.redisUrl` (string)
- `dispatch.trust.mode` (string, default "off")
- `dispatch.redis.url` (string)
- `dispatch.redis.maxLength` (number, default 100000)
- `dispatch.redis.minIdle` (string, default "60s")
- `dispatch.redis.block` (string, default "2s")
- `dispatch.redis.streamCreation` (string, default "auto")
- `dispatch.rabbitmq.url` (string)
- `dispatch.rabbitmq.exchangePrefix` (string, default "pso.")
- `dispatch.kafka.brokers` (list)
- `dispatch.kafka.clientId` (string)
- `dispatch.kafka.topicCreation` (string, default "auto")
- `dispatch.kafka.partitions` (number, default 1)
- `dispatch.kafka.replicationFactor` (number, default 1)
- `dispatch.kafka.ssl` (boolean, default false)
- `dispatch.kafka.sasl.mechanism` (string)
- `dispatch.kafka.sasl.username` (string)
- `dispatch.kafka.sasl.password` (string)

## Repository convention for validation projects

- Keep sample applications, generated projects and external validation workloads in their own directories with a README (purpose, prerequisites, how to run); they are separate from the framework packages. Cross-language contract data shared by the TypeScript and Java tests is kept next to those tests.
- Unit-test-only data stays beside its tests and is not part of a release.
- The framework (`packages/`, `pso-java/`, `scripts/`) never imports or builds code from `fixtures/`. A fixture reaches PSO only through `@pso/*` packages (`file:` path or vendored tarball) and the `pso` CLI, and keeps the workspace layout `pso create` produced.
- Fixtures are never part of a distribution or a published package, and generated artifacts, dependencies, credentials, private keys and `.env` files are never committed. `packages/cli/test/fixtures-isolation.test.ts` enforces this.
- Do not create validation projects as siblings of the repository or in a temporary directory that is later forgotten.
- Keep functional sample applications and deployment validation workloads separate and independent: a functional sample validates application behaviour (acceptance checks, Docker Compose, no server); a deployment workload validates deployment, infrastructure, operational security and remote management on a real server (declarations in `deploy/`, scenario scripts in `scenarios/`). Never copy application code between them; put a test in the one whose responsibility it belongs to. Scenario scripts read the target from environment variables such as `ECV_HOST`, `ECV_SSH_USER`, `ECV_SSH_KEY`, never from literals.
