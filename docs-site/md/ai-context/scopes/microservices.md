# Spring Boot conventions

> Java layout, naming, security and persistence rules for microservices.

## Layout and naming

Spring Boot 3.5, Java 21, Maven Wrapper (`./mvnw` or `.\mvnw.cmd`; no global Maven). Classes are PascalCase; the owning directory is the last package segment.

| Kind | Directory | File pattern |
|---|---|---|
| controller | routes/ | `*Controller.java` |
| service | application/ | `*Service.java` |
| port | ports/ | `*Port.java` |
| adapter | adapters/ | `*Adapter.java` |
| mapper | adapters/ | `*Mapper.java` |
| repository | persistence/ | `*Repository.java` |
| event | events/ | `*Event.java` |
| listener | events/ | `*Listener.java` |
| dto | contracts/ | `*Dto.java` |
| config | configs/ | `*Config.java` |
| guard | security/ | `*Guard.java` |

## Rules

- Each service is an independent Spring Boot application with its own database. Never share a database between services and never share domain entities; share versioned contracts and DTOs.
- Security is deny by default and comes from `pso-security-spring-boot-starter` (there is no SecurityConfig or JwtConfig in the project): only `/actuator/health` is public. Do not add `permitAll()` for business routes. Declare exceptions explicitly under `pso.security.rules` and review them.
- JWT settings (`pso.security.jwt.*` / `PSO_JWT_JWKS_URI`, `PSO_JWT_ISSUER`, `PSO_JWT_AUDIENCE`, `PSO_JWT_ALGORITHMS`) are validated at start-up; asymmetric algorithms only. Never invent their values.
- Configuration is typed and strict: every setting lives under `pso.*` in `application.yml` and an unknown key stops start-up. Check with `pso validate` and `pso security:validate`. Do not put credentials in YAML.
- Scopes are enforced with `@PreAuthorize("hasAuthority('SCOPE_orders:read')")`.
- Bean Validation on DTOs (`@NotBlank`, `@Valid`). Use parameterized queries.
- Persistence (`pso create service <name> --db postgres`): Flyway migrations in `src/main/resources/db/migration/` (`pso make:migration <name> --service <svc>`); migrations are immutable once applied; credentials come from `DB_URL`, `DB_USER`, `DB_PASSWORD`; tests run against an embedded PostgreSQL.
- Controllers map paths WITHOUT the service prefix (`pso.service.prefix`, derived from the name: `users-service` becomes `/users`); PSO adds it once. Never set `server.servlet.context-path`. A mapping that begins with the prefix is refused at start-up unless the repetition is intended and declared with `@RepeatsServicePrefix` (`com.pso.spring.autoconfigure.RepeatsServicePrefix`): the generators add it when the controller is named like the service (`pso make:controller orders --service orders`). `@Unprefixed` serves a controller without the prefix; use it rarely. The prefix must equal the `prefix` of the service in `pso.workspace.json` (`SPR013`).
- Lombok is available: `@RequiredArgsConstructor` for constructor injection, `@Getter @Setter @Builder @NoArgsConstructor @AllArgsConstructor` for bound request beans, records for immutable DTOs. Never `@Data` on a JPA entity; avoid `@ToString`/`@EqualsAndHashCode` on entities and on types holding secrets.
- Outbound HTTP: inject `PsoHttpClient` and call named services declared under `pso.http.client.services` (relative paths only; bearer forwarding is opt-in per service). Do not build raw `RestTemplate`/`WebClient` calls to arbitrary URLs.
- CORS: `pso.security.cors` with exact origins (never `*`); request limits under `pso.limits.*`.
- Database is off by default (`pso.database.provider: none`). Choose on purpose: `pso database:enable h2|postgresql --service <svc>`. H2 is local-only and is refused with the prod or staging profile.
- Dispatch is off by default (`pso.dispatch.enabled: false`). Install a provider with `pso dispatch:install <redis|rabbitmq|kafka> --service <name>`; the user enables it. Delivery is at-least-once (see the dispatch scope). Never add broker clients by hand. The service's tests use the in-process adapter (`pso.dispatch.provider: memory` in `application-test.yml`, from the test-scoped `pso-dispatch-test`): never set `memory` outside the test profile (`SPR015`, refused in production).
- Profiles: local, dev, test, staging, prod, h2 (no default profile). Install the PSO artifacts first with `pso java:install`.
- Generate with `pso make:controller|service|port|adapter|mapper|repository|dto|config|guard <name> --service <svc>`, `pso make:event|listener` (local application events), `pso make:dispatch-event|dispatch-handler` (distributed events), or `pso make:resource` for an endpoint across orchestrator and service. Unknown options are refused: use only the options `pso <command> --help` lists.
- Build and test: `.\mvnw.cmd test`, `pso test --service <svc>`.
