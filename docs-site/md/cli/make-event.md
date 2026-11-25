# pso make:event

> Generate a LOCAL Spring application event (a record published with ApplicationEventPublisher and delivered in the same process). For an event between services use make:dispatch-event

Generate a LOCAL Spring application event (a record published with ApplicationEventPublisher and delivered in the same process). For an event between services use make:dispatch-event.

## Usage

```text
pso make:event <name> [--service <spring-service>] [--dry-run] [--force] [--json]
```

## Flags

`--service`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso make:event order-placed --service orders --dry-run
pso make:event order-placed --service orders
```

## What it changes

- Writes `application/event/OrderPlacedEvent.java` (`events/` in a 0.1 service): a record for a LOCAL Spring application event, published with `ApplicationEventPublisher` and delivered to `@EventListener` methods of the same process.
- It never leaves the service and is not stored. An event that other services receive is a PSO Dispatch event: `pso make:dispatch-event`.

## Failure cases

- `A local application event is generated in a Spring Boot service`: the orchestrator has no local event mechanism; use `pso make:dispatch-event` there.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [microservices/hexagonal-architecture](microservices/hexagonal-architecture.md)
- [dispatch/events-and-consumers](dispatch/events-and-consumers.md)
