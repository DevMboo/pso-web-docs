# PostgreSQL CRUD in a Spring service

> A complete, tested example - connection, pool, Flyway migration, entity, repository, DTOs, mapper, service with transactions, controller, validation, errors and tests - with real output.

> [!TIP]
> This is a tested **hand-built 0.1-layout** example retained for existing projects and for learning each layer. New services generate the functional CRUD in the [official layered structure](architecture/reference-crud.md) by default; use `--template empty` to omit it. To go from an empty folder to a running manual example, follow [Build a CRUD API](tutorials/crud-api.md).

## What you will build

A `catalog` service that stores **products** in PostgreSQL and exposes them over HTTP:

| Method and path | Scope | Result |
|---|---|---|
| `POST /catalog/products` | `products:write` | `201`, the created product |
| `GET /catalog/products` | `products:read` | `200`, a list |
| `GET /catalog/products/{id}` | `products:read` | `200`, or `404` |
| `PUT /catalog/products/{id}` | `products:write` | `200`, the updated product |
| `DELETE /catalog/products/{id}` | `products:write` | `204` |

Every file shown on this page is in the framework repository (`docs/examples/spring-crud`) and is compiled and tested by the framework's own test run: a real PostgreSQL, real Flyway migrations, real JWT validation. Nothing below is pseudo-code.

## Why this shape

| Piece | Why it is separate |
|---|---|
| Controller | HTTP only: parse, validate, delegate. No business or SQL |
| Service | the **transaction boundary**; the only place that decides what happens together |
| Repository | data access; Spring Data generates it from an interface |
| Entity | how data is stored; **never** returned to clients |
| Request/response records | the public contract; change them without touching the table |
| Mapper | one visible place where fields cross the boundary |
| Flyway migration | the schema is versioned code, reviewed like any other change |

Messages from Jakarta Validation (`must not be blank`) follow the JVM language: on a Portuguese machine they come back in Portuguese (`não deve estar em branco`). Messages you write in the record, such as the `sku` pattern, are fixed. Start the JVM with `-Duser.language=en` for English, and never parse the message text: use `field` and `code`.

PSO does not require JPA. It requires only that `pso.database.provider` says what the service uses; the data starter brings JPA, Flyway and the driver, and plain JDBC (`JdbcTemplate`) works in the same service if a query is better written by hand. This page uses JPA because it is the most common starting point.

## Prerequisites

- A workspace (`pso create workspace shop`) and Java 21 with the wrapper that `pso` generates.
- For a real database: Docker, or any PostgreSQL 14 or newer. For the tests you need **neither**: they start an embedded PostgreSQL.

## 1. Create the service with PostgreSQL

```bash
pso create service catalog --db postgres
```

What was generated for persistence (besides the usual `Application.java`, the ping resource and the profiles):

```tree title="What --db postgres adds to the service"
catalog/
  pom.xml  # generated: includes pso-data-spring-boot-starter (JPA, Flyway, PostgreSQL driver)
  src/
    main/resources/
      application.yml  # generated: pso.database.provider postgresql, spring.datasource from DB_URL, DB_USER and DB_PASSWORD
      db/migration/  # generated: empty; your migrations go here (pso make:migration)
    test/java/com/pso/catalog/DatabaseMigrationTests.java  # generated: the data source answers and every migration was applied
    test/java/com/pso/catalog/TestDatabaseConfig.java  # generated: an embedded PostgreSQL for @SpringBootTest
```

An existing service can opt in with `pso database:enable postgresql --service catalog`.

## 2. Connection and pool configuration

The generated datasource reads the environment; credentials are never in a file:

```yaml title="application.yml (generated)"
spring:
  datasource:
    url: ${DB_URL:jdbc:postgresql://localhost:5432/catalog}
    username: ${DB_USER:}
    password: ${DB_PASSWORD:}
  flyway:
    enabled: true
```

Extend it with a pool and with JPA settings that make the schema the migration's job. Insert the first block under `datasource:` (after `password`) and the second **next to** `flyway:`: `datasource`, `flyway` and `jpa` are siblings under `spring:`, so `jpa:` is not nested inside `flyway:`:

```yml
// spring-crud/datasource-pool.yml
    hikari:                         # connection pool (HikariCP is Spring Boot's default)
      maximum-pool-size: ${DB_POOL_SIZE:10}   # size it from the database's connection limit divided by the number of instances
      minimum-idle: ${DB_POOL_MIN_IDLE:2}
      connection-timeout: 5000      # ms to wait for a free connection before failing fast
      max-lifetime: 1800000         # ms; keep it shorter than any idle-connection timeout in front of the database
```

```yml
// spring-crud/jpa.yml
  jpa:
    open-in-view: false             # no database access while the response is being rendered
    hibernate:
      ddl-auto: validate            # Flyway owns the schema; Hibernate only checks that entities match it
    properties:
      hibernate:
        jdbc:
          time_zone: UTC
```

The result has this shape (an excerpt of the complete file in [Build a CRUD API](tutorials/crud-api.md); values and comments omitted):

```yaml title="application.yml (structure of the spring: block)"
spring:
  datasource:
    url: ...
    username: ...
    password: ...
    hikari: { ... }
  flyway:
    enabled: true
  jpa:
    open-in-view: false
    hibernate:
      ddl-auto: validate
```

| Setting | Why |
|---|---|
| `ddl-auto: validate` | Hibernate checks entities against the schema and **never changes it**. Flyway is the only writer of DDL |
| `open-in-view: false` | no queries while the response is rendered; a lazy-loading mistake fails in the service instead of leaking a connection |
| `maximum-pool-size` | the database has a connection limit. Rule of thumb: `limit / number of instances`, minus a margin for tools. The default of 10 is a starting point, not a recommendation |
| `connection-timeout` | when the pool is exhausted, fail in 5 s with a clear error instead of queueing requests behind a stuck query |
| `max-lifetime` | recycle connections before a firewall or proxy silently drops idle ones |

Set the real values per environment:

```bash
export DB_URL=jdbc:postgresql://db.internal:5432/catalog
export DB_USER=catalog_app
export DB_PASSWORD="$(vault read ...)"      # from your secret manager; never commit it
```

### A local PostgreSQL with Docker

```bash
docker run -d --name catalog-db -e POSTGRES_USER=catalog -e POSTGRES_PASSWORD=catalog-dev -e POSTGRES_DB=catalog -p 127.0.0.1:5432:5432 postgres:16-alpine
export DB_URL=jdbc:postgresql://127.0.0.1:5432/catalog DB_USER=catalog DB_PASSWORD=catalog-dev
```

The password above is a throwaway for a container that listens on loopback only. Do not reuse it anywhere.

## 3. The migration

```sql
// spring-crud/src/main/resources/db/migration/V1__create_products.sql
-- Migrations are append-only: never edit an applied version, add a new one (V2__..., V3__...).
CREATE TABLE products (
    id          UUID PRIMARY KEY,
    sku         VARCHAR(32)  NOT NULL,
    name        VARCHAR(120) NOT NULL,
    price_cents BIGINT       NOT NULL CHECK (price_cents > 0),
    created_at  TIMESTAMPTZ  NOT NULL DEFAULT now(),
    updated_at  TIMESTAMPTZ  NOT NULL DEFAULT now(),
    version     BIGINT       NOT NULL DEFAULT 0,
    CONSTRAINT uq_products_sku UNIQUE (sku)
);
```

Rules that keep migrations safe:

- Never edit a migration that has been applied anywhere. Flyway stores its checksum and refuses to start if it changed. Add `V2__...` instead.
- Put constraints in the database (`NOT NULL`, `CHECK`, `UNIQUE`). The application validates for good messages; the database is the last line of defence when two requests race.
- In production, make destructive changes in two releases: first stop using a column, later drop it. Add columns as nullable or with a default so the old version keeps working during a rolling deploy.
- Large data changes belong in their own migration, tried against a copy of production data first.

## 4. The entity and the repository

```java
// spring-crud/src/main/java/com/pso/catalog/domain/Product.java
package com.pso.catalog.domain;

import jakarta.persistence.Column;
import jakarta.persistence.Entity;
import jakarta.persistence.Id;
import jakarta.persistence.Table;
import jakarta.persistence.Version;
import java.time.Instant;
import java.util.UUID;

/** Persistence model. It is never returned from a controller: the API contract is ProductResponse. */
@Entity
@Table(name = "products")
public class Product {

    @Id
    private UUID id;

    @Column(nullable = false, length = 32, updatable = false)
    private String sku;

    @Column(nullable = false, length = 120)
    private String name;

    @Column(name = "price_cents", nullable = false)
    private long priceCents;

    @Column(name = "created_at", nullable = false, updatable = false)
    private Instant createdAt;

    @Column(name = "updated_at", nullable = false)
    private Instant updatedAt;

    @Version
    private long version;

    protected Product() {
        // required by JPA
    }

    public Product(String sku, String name, long priceCents) {
        this.id = UUID.randomUUID();
        this.sku = sku;
        this.name = name;
        this.priceCents = priceCents;
        this.createdAt = Instant.now();
        this.updatedAt = this.createdAt;
    }

    public void change(String name, long priceCents) {
        this.name = name;
        this.priceCents = priceCents;
        this.updatedAt = Instant.now();
    }

    public UUID getId() {
        return id;
    }

    public String getSku() {
        return sku;
    }

    public String getName() {
        return name;
    }

    public long getPriceCents() {
        return priceCents;
    }

    public Instant getCreatedAt() {
        return createdAt;
    }

    public Instant getUpdatedAt() {
        return updatedAt;
    }
}
```

- The constructor you call creates the id (a UUID), so the id is known before the insert and there is no database round trip to learn it.
- `@Version` gives **optimistic locking**: two concurrent updates of the same row cannot silently overwrite each other; the second fails.
- `sku` is `updatable = false`: the entity itself refuses to change it.

```java
// spring-crud/src/main/java/com/pso/catalog/persistence/ProductRepository.java
package com.pso.catalog.persistence;

import com.pso.catalog.domain.Product;
import java.util.UUID;
import org.springframework.data.jpa.repository.JpaRepository;

/** Spring Data generates the implementation. Derived queries are parameterised: values are never concatenated into SQL. */
public interface ProductRepository extends JpaRepository<Product, UUID> {

    boolean existsBySku(String sku);
}
```

`existsBySku` is a **derived query**: Spring Data turns it into a parameterised statement. Values are always bound parameters, so there is no string concatenation and no SQL injection. If you write a query by hand, use `@Query` with named parameters (`:sku`) or `JdbcTemplate` with `?`; never build SQL with `+` from request data.

## 5. The contract: requests, response and mapper

```java
// spring-crud/src/main/java/com/pso/catalog/contracts/ProductRequest.java
package com.pso.catalog.contracts;

import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.Pattern;
import jakarta.validation.constraints.Positive;
import jakarta.validation.constraints.Size;

/**
 * What a client may send. The sku is part of the create request only: it identifies the product and cannot be changed afterwards,
 * so the update request leaves it out (an unknown "sku" in an update is ignored by Jackson's defaults in Spring Boot, never applied).
 */
public record ProductRequest(
        @NotBlank @Pattern(regexp = "[A-Z0-9-]{3,32}", message = "must be 3-32 characters: A-Z, 0-9 or -") String sku,
        @NotBlank @Size(max = 120) String name,
        @Positive long priceCents) {
}
```

```java
// spring-crud/src/main/java/com/pso/catalog/contracts/ProductUpdate.java
package com.pso.catalog.contracts;

import jakarta.validation.constraints.NotBlank;
import jakarta.validation.constraints.Positive;
import jakarta.validation.constraints.Size;

/** The fields a client may change on an existing product. */
public record ProductUpdate(@NotBlank @Size(max = 120) String name, @Positive long priceCents) {
}
```

```java
// spring-crud/src/main/java/com/pso/catalog/contracts/ProductResponse.java
package com.pso.catalog.contracts;

import java.time.Instant;
import java.util.UUID;

/** The public contract. Internal columns (version) are deliberately absent. */
public record ProductResponse(UUID id, String sku, String name, long priceCents, Instant createdAt, Instant updatedAt) {
}
```

```java
// spring-crud/src/main/java/com/pso/catalog/adapters/ProductMapper.java
package com.pso.catalog.adapters;

import com.pso.catalog.contracts.ProductRequest;
import com.pso.catalog.contracts.ProductResponse;
import com.pso.catalog.domain.Product;
import org.springframework.stereotype.Component;

/** Hand-written mapping keeps the boundary explicit: every field that crosses it is a decision, not a side effect of reflection. */
@Component
public class ProductMapper {

    public Product toEntity(ProductRequest request) {
        return new Product(request.sku(), request.name().strip(), request.priceCents());
    }

    public ProductResponse toResponse(Product p) {
        return new ProductResponse(p.getId(), p.getSku(), p.getName(), p.getPriceCents(), p.getCreatedAt(), p.getUpdatedAt());
    }
}
```

Why not return the entity? Because the entity is the table. If the entity were the API, adding a column would change what clients receive, a lazy relation could trigger queries during serialization, and internal columns (`version`) would leak. Records make every exposed field a decision.

## 6. The service: transactions

```java
// spring-crud/src/main/java/com/pso/catalog/application/ProductService.java
package com.pso.catalog.application;

import com.pso.catalog.adapters.ProductMapper;
import com.pso.catalog.contracts.ProductRequest;
import com.pso.catalog.contracts.ProductResponse;
import com.pso.catalog.contracts.ProductUpdate;
import com.pso.catalog.domain.Product;
import com.pso.catalog.persistence.ProductRepository;
import java.util.List;
import java.util.UUID;
import org.springframework.http.HttpStatus;
import org.springframework.stereotype.Service;
import org.springframework.transaction.annotation.Transactional;
import org.springframework.web.server.ResponseStatusException;

/**
 * The transaction boundary. Each public method is one unit of work: either every change commits, or none does.
 * Reads are marked readOnly so the connection can skip dirty checking.
 */
@Service
public class ProductService {

    private final ProductRepository products;
    private final ProductMapper mapper;

    public ProductService(ProductRepository products, ProductMapper mapper) {
        this.products = products;
        this.mapper = mapper;
    }

    @Transactional
    public ProductResponse create(ProductRequest request) {
        if (products.existsBySku(request.sku())) {
            throw new ResponseStatusException(HttpStatus.CONFLICT, "a product with this sku already exists");
        }
        return mapper.toResponse(products.save(mapper.toEntity(request)));
    }

    @Transactional(readOnly = true)
    public List<ProductResponse> list() {
        return products.findAll().stream().map(mapper::toResponse).toList();
    }

    @Transactional(readOnly = true)
    public ProductResponse get(UUID id) {
        return mapper.toResponse(find(id));
    }

    @Transactional
    public ProductResponse update(UUID id, ProductUpdate update) {
        Product product = find(id);
        product.change(update.name().strip(), update.priceCents()); // dirty checking writes the UPDATE when the transaction commits
        return mapper.toResponse(product);
    }

    @Transactional
    public void delete(UUID id) {
        products.delete(find(id));
    }

    private Product find(UUID id) {
        return products.findById(id).orElseThrow(() -> new ResponseStatusException(HttpStatus.NOT_FOUND));
    }
}
```

- Each public method is **one transaction**. `create` checks for a duplicate and inserts; if the insert throws, nothing is stored.
- `update` does not call `save`: the entity is *managed* inside the transaction, so changing it is enough; Hibernate writes one `UPDATE` at commit (dirty checking).
- Reads use `readOnly = true`.
- Failures the caller can fix are `ResponseStatusException`s: PSO turns them into Problem Details with the status you chose (`404` becomes `PSO_HTTP_NOT_FOUND`, `409` becomes `PSO_HTTP_ERROR_409`).
- The `existsBySku` check gives a friendly `409`. Two simultaneous requests can both pass it; the `UNIQUE` constraint then rejects the second insert. Handle that rare case at the edge if you need a `409` there too, or accept the generic error.

## 7. The controller

```java
// spring-crud/src/main/java/com/pso/catalog/routes/ProductController.java
package com.pso.catalog.routes;

import com.pso.catalog.application.ProductService;
import com.pso.catalog.contracts.ProductRequest;
import com.pso.catalog.contracts.ProductResponse;
import com.pso.catalog.contracts.ProductUpdate;
import jakarta.validation.Valid;
import java.util.List;
import java.util.UUID;
import org.springframework.http.HttpStatus;
import org.springframework.security.access.prepost.PreAuthorize;
import org.springframework.web.bind.annotation.DeleteMapping;
import org.springframework.web.bind.annotation.GetMapping;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.PostMapping;
import org.springframework.web.bind.annotation.PutMapping;
import org.springframework.web.bind.annotation.RequestBody;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseStatus;
import org.springframework.web.bind.annotation.RestController;

/** HTTP only: parse, validate, delegate. The service prefix (/catalog) is applied by PSO, so mappings do not repeat it. */
@RestController
@RequestMapping("/products")
public class ProductController {

    private final ProductService service;

    public ProductController(ProductService service) {
        this.service = service;
    }

    @GetMapping
    @PreAuthorize("hasAuthority('SCOPE_products:read')")
    public List<ProductResponse> list() {
        return service.list();
    }

    @GetMapping("/{id}")
    @PreAuthorize("hasAuthority('SCOPE_products:read')")
    public ProductResponse get(@PathVariable UUID id) {
        return service.get(id);
    }

    @PostMapping
    @ResponseStatus(HttpStatus.CREATED)
    @PreAuthorize("hasAuthority('SCOPE_products:write')")
    public ProductResponse create(@Valid @RequestBody ProductRequest request) {
        return service.create(request);
    }

    @PutMapping("/{id}")
    @PreAuthorize("hasAuthority('SCOPE_products:write')")
    public ProductResponse update(@PathVariable UUID id, @Valid @RequestBody ProductUpdate update) {
        return service.update(id, update);
    }

    @DeleteMapping("/{id}")
    @ResponseStatus(HttpStatus.NO_CONTENT)
    @PreAuthorize("hasAuthority('SCOPE_products:write')")
    public void delete(@PathVariable UUID id) {
        service.delete(id);
    }
}
```

- `@Valid` turns on Jakarta Validation for the body; a violation never reaches the service.
- `@PreAuthorize("hasAuthority('SCOPE_products:read')")` requires the token scope. Everything not explicitly allowed requires authentication.
- Paths do not include `/catalog`: PSO adds the service prefix once.
- To expose the service through the gateway, add the routes there ([first microservice](getting-started/first-microservice.md)).

## 8. Run it and see what happens

```bash
cd catalog
./mvnw spring-boot:run -Dspring-boot.run.profiles=local
```

The start-up log shows the pool, the migrations and the JPA context (Expected output; the first run against an empty database):

```text evidence="expected"
INFO [catalog,,] ... com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Start completed.
INFO [catalog,,] ... org.flywaydb.core.FlywayExecutor         : Database: jdbc:postgresql://127.0.0.1:55432/docs_catalog (PostgreSQL 16.15)
INFO [catalog,,] ... o.f.core.internal.command.DbMigrate      : Migrating schema "public" to version "1 - create products"
INFO [catalog,,] ... o.f.core.internal.command.DbMigrate      : Successfully applied 1 migration to schema "public", now at version v1
INFO [catalog,,] ... j.LocalContainerEntityManagerFactoryBean : Initialized JPA EntityManagerFactory for persistence unit 'default'
INFO [catalog,,] ... com.pso.catalog.Application              : Started Application in 4.305 seconds
```

Verify in the database:

```text evidence="expected"
 public | flyway_schema_history | table | pso
 public | products              | table | pso

 version |   description   | success
---------+-----------------+---------
 1       | create products | t
```

Without a token the service answers `401` and touches nothing:

```text evidence="expected"
HTTP/1.1 401
X-Correlation-ID: bfb268c4-8017-4269-bda7-d083bc332384
WWW-Authenticate: Bearer
```

With a valid token (issued by your identity provider; see [security](spring/security-and-cors.md)):

```bash
curl -i -X POST http://127.0.0.1:8081/catalog/products \
  -H "authorization: Bearer $TOKEN" -H "content-type: application/json" \
  -d '{"sku":"KB-100","name":"Keyboard","priceCents":4999}'
```

```text evidence="expected"
HTTP/1.1 201
content-type: application/json

{"id":"6b0f3f5c-1d0e-4b84-9f0a-2b6d5e0c7a31","sku":"KB-100","name":"Keyboard","priceCents":4999,"createdAt":"2026-11-08T14:30:01.512Z","updatedAt":"2026-11-08T14:30:01.512Z"}
```

(Your id and timestamps differ.) The `version` column does not appear: it is not in `ProductResponse`.

An invalid body, a missing product and a duplicate (each answer is `application/problem+json`; the test run asserts these exact codes):

```bash
curl -s -X POST .../catalog/products -H "authorization: Bearer $TOKEN" -H "content-type: application/json" -d '{"sku":"bad sku!","name":"","priceCents":-5}'
```

```json
{ "type": "urn:pso:problem:validation-failed", "status": 400, "code": "PSO_VALIDATION_FAILED",
  "errors": [ { "field": "sku", "message": "must be 3-32 characters: A-Z, 0-9 or -" },
              { "field": "name", "message": "must not be blank" },
              { "field": "priceCents", "message": "must be greater than 0" } ],
  "correlationId": "..." }
```

`GET` of an id that does not exist is `404` with `"code": "PSO_HTTP_NOT_FOUND"`; creating `KB-100` twice is `409` with `"code": "PSO_HTTP_ERROR_409"` and the detail `a product with this sku already exists`.

## 9. The tests

```bash
./mvnw test
```

The tests (`ProductCrudTests`) run against a real PostgreSQL started in-process (no Docker needed), apply the real Flyway migrations and validate real signed JWTs. They check:

| Test | What it proves |
|---|---|
| `flywayAppliedTheProductsMigration` | version 2 ran |
| `createReadUpdateDeleteRoundTrip` | the happy path, the database row after the update, `version` never exposed, `404` after delete |
| `invalidInputIsA400ProblemWithFieldErrorsAndNothingIsStored` | validation rejects and the row count is unchanged |
| `aDuplicateSkuIsA409...` | the friendly error, and that the UNIQUE constraint rejects a direct insert |
| `scopesAreEnforcedBeforeAnythingTouchesTheDatabase` | `403`/`401` and no row created by a read-only token |
| `aFailureInsideATransactionRollsEverythingBack` | `ROLL-1` was inserted, then an exception occurred, and the row is gone |

Result of the framework's run of this exact example: `Tests run: 6, Failures: 0, Errors: 0, Skipped: 0`.

## 10. Variations

- **Pagination**: change `list()` to take `Pageable` and return `Page<ProductResponse>` (`products.findAll(pageable).map(mapper::toResponse)`). Always bound the page size.
- **JDBC instead of JPA**: inject `JdbcTemplate`, keep the same service/transaction structure, write SQL with `?` parameters.
- **More tables**: one migration per change, one entity and repository per aggregate, and keep transactions in services.
- **Events**: see the next section.

## 11. Publishing an event in the same transaction (outbox)

If other services must learn about a new product, do not publish after the save and hope. Store the event in the same transaction:

```bash
pso dispatch:install redis --service catalog
pso dispatch:outbox --service catalog
```

```java
@Transactional
public ProductResponse create(ProductRequest request) {
    ...
    Product saved = products.save(mapper.toEntity(request));
    outbox.publish("catalog.product-created", Map.of("productId", saved.getId().toString()));   // rolls back with the product
    return mapper.toResponse(saved);
}
```

Delivery is at-least-once, so consumers must tolerate a repeated event. Details: [transactional outbox](dispatch/outbox.md).

## 12. Production considerations

- **Credentials**: environment variables or your secret manager only. The `prod` profile refuses H2.
- **Pool**: set `DB_POOL_SIZE` from the database limit and the instance count; watch pool wait time in [metrics](devops/observability.md).
- **Migrations**: run on start-up for a single instance; for many instances prefer a one-off migration job before the rollout. Never use `ddl-auto: update`.
- **Backups and restore tests** are yours; PSO does not manage the database.
- **Timeouts**: a slow query holds a pooled connection. Add statement timeouts at the database role (`ALTER ROLE catalog_app SET statement_timeout = '5s'`).
- **Least privilege**: the application role needs DML on its tables; use a separate, more privileged role for migrations if your policy requires it.

## Anti-patterns

| Do not | Do instead |
|---|---|
| return the entity from a controller | return a response record |
| concatenate request data into SQL | derived queries, `@Query` with named parameters, `JdbcTemplate` with `?` |
| `ddl-auto: update` or `create` | Flyway migrations and `validate` |
| edit an applied migration | add a new version |
| put `@Transactional` on controllers or repositories | on service methods |
| read `DB_PASSWORD` into a file or image | inject it at run time |

## Troubleshooting

| Symptom | Cause and fix |
|---|---|
| `FlywayValidateException: Validate failed ... checksum mismatch` | an applied migration was edited; restore it and add a new version |
| `Schema-validation: missing table [products]` | the migration did not run (`flyway.enabled`, wrong `DB_URL`) or the entity name differs from the table |
| `Connection to ... refused` at start | the database is not reachable at `DB_URL`; check the container and the port |
| `HikariPool-1 - Connection is not available, request timed out after 5000ms` | the pool is exhausted: slow queries, a transaction left open, or too small a pool |
| `403` on a call that worked yesterday | the token no longer carries `products:write`; check scopes, not the database |
| `PSO_VALIDATION_FAILED` | read the `errors` list; each entry names the field |

## Next steps

- [Database selection and H2](spring/database-and-h2.md)
- [Transactional outbox](dispatch/outbox.md)
- [Logging](devops/logging.md) and [metrics](devops/observability.md)
