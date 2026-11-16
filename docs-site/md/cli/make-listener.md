# pso make:listener

> Generate a LOCAL Spring @EventListener for the application event of the same name (same process). For an event from another service use make:dispatch-handler

Generate a LOCAL Spring @EventListener for the application event of the same name (same process). For an event from another service use make:dispatch-handler.

## Usage

```text
pso make:listener <name> [--service <spring-service>] [--dry-run] [--force] [--json]
```

## Flags

`--service`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso make:listener order-placed --service orders --dry-run
pso make:listener order-placed --service orders
```

## What it changes

- Writes `events/OrderPlacedListener.java`: a component with an `@EventListener` method for the LOCAL application event of the same name (typed with `OrderPlacedEvent` when `pso make:event order-placed` created it).
- It is not a Dispatch consumer: events of other services are handled by a `@PsoEventHandler` method, generated with `pso make:dispatch-handler`.

## Failure cases

- Without the event class the method takes `Object` and a comment names the command that creates the event.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [microservices/hexagonal-architecture](microservices/hexagonal-architecture.md)
- [dispatch/events-and-consumers](dispatch/events-and-consumers.md)
