# First microservice

> Generate a Spring Boot service that is secure from its first request, run its tests, start it, and read what it answers with and without a token.

## 1. Command

```pso-example
pso create workspace demo
cd demo
pso create service catalog
pso validate
```

Use `pso create service orders --db postgres` for a service with PostgreSQL and Flyway ([PostgreSQL CRUD](spring/postgresql-crud.md)).

## 2. What was created

Expected output:

```text evidence="expected"
$ pso create service catalog
created    pom.xml
created    mvnw
created    mvnw.cmd
created    .mvn/wrapper/maven-wrapper.properties
created    lombok.config
created    src/main/java/com/pso/catalog/Application.java
created    src/main/java/com/pso/catalog/application/PingService.java
created    src/main/java/com/pso/catalog/contracts/PingDto.java
created    src/main/java/com/pso/catalog/contracts/PingRequest.java
created    src/main/java/com/pso/catalog/routes/PingController.java
created    src/main/resources/application.yml
created    src/main/resources/application-{dev,h2,local,prod,staging,test}.yml
created    src/test/java/com/pso/catalog/ApplicationTests.java
created    src/test/java/com/pso/catalog/application/PingServiceTests.java
created    src/test/java/com/pso/catalog/routes/PingControllerTests.java
registered "catalog" in pso.workspace.json (port 8081)
```

(The per-profile files are listed as one line here; the command prints each.)

| Path | Purpose |
|---|---|
| `pom.xml` | Spring Boot 3.5 parent, the PSO BOM and starters, `java.version` 21 |
| `mvnw`, `mvnw.cmd`, `.mvn/wrapper/` | Maven Wrapper (Maven 3.9.9, checksum pinned): no `mvn` needed |
| `Application.java` | the Spring Boot entry point |
| `routes/PingController.java` | an example controller, served under the service prefix |
| `application/PingService.java` | constructor injection with Lombok |
| `contracts/` | request and response records |
| `application.yml`, `application-<profile>.yml` | configuration; no default profile on purpose: choose one (`local`, `dev`, `test`, `staging`, `prod`, `h2`) |
| `src/test/...` | unit and integration tests with real JWT validation |

## 3. The generated controller

```java title="catalog/src/main/java/com/pso/catalog/routes/PingController.java" {8-10}
@RestController
@RequestMapping("/ping")
@RequiredArgsConstructor
public class PingController {

    private final PingService service;

    @PostMapping
    @PreAuthorize("hasAuthority('SCOPE_ping:write')")
    public PingDto ping(@Valid @RequestBody PingRequest request) {
        return service.ping(request);
    }
}
```

There is no security code in the project: the [security starter](spring/security-and-cors.md) provides a deny-by-default chain. Only the health endpoints are public; every other route needs a valid JWT, and scopes become `SCOPE_*` authorities for `@PreAuthorize`. The controller maps `/ping` and PSO serves it under the [service prefix](spring/service-prefix.md) (`/catalog/ping`). Without `PSO_JWT_*` settings every token is rejected.

> [!NOTE]
> The service resolves the PSO Java artifacts (`com.pso:*`) from your local Maven repository. If you have not done it on this machine yet, run `pso java:install` once before the next step ([installation](getting-started/installation.md)); otherwise Maven stops with `Non-resolvable import POM: com.pso:pso-bom`.

## 4. Run the tests

```powershell tab="PowerShell" group="run"
cd catalog
.\mvnw.cmd test
```

```bash tab="macOS / Linux" group="run"
cd catalog
./mvnw test
```

Expected output of the second run (expected). Here the service also has a `product` resource added by `pso make:resource`:

```text evidence="expected"
[INFO] Running com.pso.catalog.application.PingServiceTests
[INFO] Tests run: 1, Failures: 0, Errors: 0, Skipped: 0 -- in com.pso.catalog.application.PingServiceTests
[INFO] Running com.pso.catalog.ApplicationTests
[INFO] Tests run: 6, Failures: 0, Errors: 0, Skipped: 0 -- in com.pso.catalog.ApplicationTests
[INFO] Running com.pso.catalog.routes.PingControllerTests
[INFO] Tests run: 6, Failures: 0, Errors: 0, Skipped: 0 -- in com.pso.catalog.routes.PingControllerTests
[INFO] Running com.pso.catalog.routes.ProductControllerTests
[INFO] Tests run: 3, Failures: 0, Errors: 0, Skipped: 0 -- in com.pso.catalog.routes.ProductControllerTests
[INFO] Tests run: 16, Failures: 0, Errors: 0, Skipped: 0
[INFO] BUILD SUCCESS
```

Without the extra resource a fresh service runs 13 tests (the 16 above minus `ProductControllerTests`). What they prove: health is public, everything else answers `401` without a token, a token with the scope succeeds, a token without it is `403`, an expired token is `401`, invalid bodies are `400` with field errors, and the unprefixed path does not exist.

## 5. Start it and call it

```powershell
cd catalog
.\mvnw.cmd spring-boot:run "-Dspring-boot.run.profiles=local"
```

End of start-up with the local profile (Expected output from a generated service named `orders` that was started on port 18123; yours shows `catalog` and port 8081):

```text evidence="expected"
2026-11-08T11:21:55.402-03:00  INFO [orders,,] 40436 --- [orders] [           main] o.s.b.w.embedded.tomcat.TomcatWebServer  : Tomcat started on port 18123 (http) with context path '/'
2026-11-08T11:21:55.413-03:00  INFO [orders,,] 40436 --- [orders] [           main] com.pso.orders.Application               : Started Application in 2.578 seconds (process running for 2.889)
```

Without a token the service answers `401` and sets `WWW-Authenticate: Bearer`; the `X-Correlation-ID` header is the id that appears in its logs ([logging](devops/logging.md)). With `PSO_JWT_*` set to your issuer's values and a token carrying `ping:write`:

```bash
curl -i -X POST http://127.0.0.1:8081/catalog/ping -H "authorization: Bearer $TOKEN" -H "content-type: application/json" -d '{"message":"hello"}'
```

*(expected output: `200` with `{"service":"catalog","message":"hello"}`; the generated test asserts exactly the `service` and `message` members)*

## 6. Variations

| You want | Do |
|---|---|
| a database | `pso create service orders --db postgres`, or `pso database:enable postgresql --service catalog` |
| to run on another port | `pso create service orders --port 8090` |
| JSON logs | the `prod` and `staging` profiles write structured JSON; see [Logging](devops/logging.md) |
| messaging | `pso dispatch:install redis --service catalog` ([Dispatch](dispatch/installation.md)) |
| a resource exposed through the gateway | `pso make:resource` ([First endpoint](getting-started/first-endpoint.md)) |

## 7. Common mistakes

- Expecting `mvn` on the PATH: use the wrapper (`mvnw`).
- Calling the service with `curl` and no token: `401` is correct, not a bug.
- Putting business routes in `permitAll()`: `pso security:check` flags anything beyond health as a finding.
- Running without choosing a profile and wondering why the prod-only rules are absent: pick one explicitly.
- Forgetting that the service prefix is applied by PSO: map `/ping`, not `/catalog/ping`.

## Troubleshooting

| Symptom | Cause | Check | Fix |
|---|---|---|---|
| the service stops with `pso.security.jwt.jwks-uri is required` | Spring needs `PSO_JWT_JWKS_URI`; a key file is read by the orchestrator only | the first lines of the start-up log | set `PSO_JWT_JWKS_URI` (https, or http on loopback in development) |
| `Port 8081 was already in use` | another process holds the port | the OS's port listing | stop it or change `port` in `pso.workspace.json` |
| `PSO_MISPLACED_RESOURCE` after adding a class | the class is outside the directory its kind requires | `pso validate` | move it |
| `401` from the service | no token, or issuer or audience differ | `pso security:validate` | align the values with the token |

The first row was reproduced while writing [Build a CRUD API](tutorials/crud-api.md).

## Next steps

- [First endpoint](getting-started/first-endpoint.md)
- [PostgreSQL CRUD](spring/postgresql-crud.md)
- [Project structure](microservices/project-structure.md)
