# Java project structure

> The simplified hexagonal layout and the naming rules PSO validates, with the generators that place each kind of class and what they create.

```tree title="The simplified hexagonal layout"
catalog/
  pom.xml  # generated
  mvnw  # generated: Maven Wrapper, with mvnw.cmd and .mvn/wrapper/maven-wrapper.properties
  lombok.config  # generated
  src/
    main/
      java/com/pso/catalog/
        Application.java  # generated
        configs/  # application-specific configuration (security comes from the starter): ...Config.java
        routes/  # controllers: ...Controller.java
        application/  # services: ...Service.java
        ports/  # interfaces the application needs: ...Port.java
        adapters/  # implementations of ports and mappers: ...Adapter.java, ...Mapper.java
        persistence/  # repositories: ...Repository.java
        events/  # ...Event.java, ...Listener.java
        contracts/  # request and response records: ...Dto.java
        security/  # ...Guard.java
      resources/
        application.yml  # generated, plus application-<profile>.yml
        contracts/  # optional: *.contract.json
    test/java/com/pso/catalog/  # generated: unit and integration tests
```

The owning directory is the **last package segment**, and the package prefix (`com.pso.catalog`) is free. Component scanning and auto-configuration behave exactly as in any Spring Boot application.

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

Plain helper files without a recognised suffix are allowed anywhere and are ignored by discovery.

## The generators

`pso make:controller|service|port|adapter|mapper|repository|event|listener|dto|config|guard <name> --service catalog`. Names are given in kebab-case and become PascalCase classes with the right suffix. Real output (verified):

```text evidence="verified"
$ pso make:dto customer-request --service catalog
created    src/main/java/com/pso/catalog/contracts/CustomerRequestDto.java
$ pso make:service pricing --service catalog
created    src/main/java/com/pso/catalog/application/PricingService.java
$ pso make:port payment-gateway --service catalog
created    src/main/java/com/pso/catalog/ports/PaymentGatewayPort.java
$ pso make:adapter payment-gateway --service catalog
created    src/main/java/com/pso/catalog/adapters/PaymentGatewayAdapter.java
$ pso make:mapper customer --service catalog
created    src/main/java/com/pso/catalog/adapters/CustomerMapper.java
$ pso make:repository customer --service catalog
created    src/main/java/com/pso/catalog/persistence/CustomerRepository.java
```

The generated DTO is a record with a validation starting point; make it yours:

```java
package com.pso.catalog.contracts;

import jakarta.validation.constraints.NotBlank;

public record CustomerRequestDto(@NotBlank String id) {
}
```

A class with a known suffix in the wrong directory is `PSO_MISPLACED_RESOURCE`; a badly named one is `PSO_BAD_NAME`.

## Where does this code go?

| You are writing | Put it in | Why |
|---|---|---|
| an HTTP endpoint | `routes/` (`@RestController`) | translates HTTP to a call |
| the rule behind it | `application/` (`@Service`) | transaction boundary, reusable |
| a request or response shape | `contracts/` (records) | the public contract; never an entity |
| something the service needs from outside (payments, a remote API) | `ports/` interface and `adapters/` implementation | the service stays testable without the outside |
| persistence | `persistence/` (Spring Data repository) and entities where you keep your domain | see [PostgreSQL CRUD](spring/postgresql-crud.md) |
| converting between shapes | a mapper in `adapters/` | one visible place for field mapping |
| an event | `events/` | [Dispatch](dispatch/events-and-consumers.md) |

The tested [PostgreSQL CRUD example](spring/postgresql-crud.md) uses `domain/` for entities and repositories; that directory has no framework meaning (it is a helper location the discovery ignores), which is allowed.

## Common mistakes

- Putting a `Dto` in `routes/` (misplaced) or an entity in `contracts/`.
- Making a port for everything. Add one where there is a boundary (a remote system) worth substituting in tests.
- Adding configuration classes that duplicate what the starters already configure.

## Next steps

- [Hexagonal architecture](microservices/hexagonal-architecture.md)
- [PostgreSQL CRUD](spring/postgresql-crud.md)
