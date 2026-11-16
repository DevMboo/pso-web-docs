# Lombok conventions

> How Lombok is configured in generated services and which annotations belong where.

Every generated Spring service includes Lombok. The version is the one Spring Boot manages, compiled with `release 21`. The `pom.xml` declares the dependency as `provided`, configures the annotation processor path explicitly and excludes Lombok from the packaged jar; `lombok.config` marks generated code so tools can skip it.

| Use | Annotation |
|---|---|
| services and controllers (constructor injection) | `@RequiredArgsConstructor` on a class whose dependencies are `final` fields |
| mutable request beans bound by Jackson | `@Getter @Setter @NoArgsConstructor @AllArgsConstructor @Builder` (see `PingRequest`) |
| immutable DTOs and responses | a Java **record**; no Lombok needed (see `PingDto`) |

## Rules

- **Never `@Data` on a JPA entity.** Its generated `equals`, `hashCode` and `toString` use mutable state and relationships.
- Avoid `@ToString` on anything that holds secrets or entity relationships: it can leak values or recurse.
- Avoid `@EqualsAndHashCode` on mutable entities unless you define identity semantics explicitly (for example by id only).
- Prefer records for DTOs; reserve Lombok for beans that frameworks need to mutate.
- Use constructor injection for services. `pso make:service` generates a `@Service @RequiredArgsConstructor` class.

The generated tests check that the template follows these rules (no `@Data` or `@ToString` defaults).

## What it looks like

`pso make:service pricing --service catalog` prints and creates:

```text evidence="verified"
created    src/main/java/com/pso/catalog/application/PricingService.java
```

```java title="catalog/src/main/java/com/pso/catalog/application/PricingService.java (generated)"
package com.pso.catalog.application;

import lombok.RequiredArgsConstructor;
import org.springframework.stereotype.Service;

/** Dependencies are final fields: Lombok generates the constructor and Spring injects through it. */
@Service
@RequiredArgsConstructor
public class PricingService {
}
```

Add your dependencies as `private final` fields and the generated constructor injects them. The generated DTOs (`PingDto`) are records.

## Common mistakes

- `@Data` on a JPA entity.
- `@ToString` on a class that holds a token or an entity relationship.
- Lombok on records (not needed).

## Next steps

- [Java project structure](microservices/project-structure.md)
- [PostgreSQL CRUD](spring/postgresql-crud.md) (entities written by hand, no `@Data`)
