# pso make:dispatch-handler

> Generate a declared consumer of a PSO Dispatch event that this project already declares. Spring: a @PsoEventHandler component; orchestrator: a @DispatchConsumer class listed in EVENTS.pso.ts

Generate a declared consumer of a PSO Dispatch event that this project already declares. Spring: a @PsoEventHandler component; orchestrator: a @DispatchConsumer class listed in EVENTS.pso.ts.

## Usage

```text
pso make:dispatch-handler <event.name> [--service <name>] [--group <consumer-group>] [--module <dir>] [--dry-run] [--force] [--json]
```

## Flags

`--service`, `--group`, `--module`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso make:dispatch-handler orders.placed --service billing
pso make:dispatch-handler orders.placed --service gateway --group gateway-notifications
```

## What it changes

- Generates a consumer of an event the project already declares.
- Spring Boot: `events/OrdersPlacedHandler.java`, a component with a `@PsoEventHandler("orders.placed")` method.
- Orchestrator: a `@DispatchConsumer` class in `src/dispatch/<module>/`, listed in `consumers` of `EVENTS.pso.ts` (a consumer that is not listed never runs).

## Verify the result

- `pso dispatch:consumers` lists the consumer; `pso dispatch:validate` checks it against the declaration.

## Failure cases

- `"orders.placed" is not declared in this service`: declare it first with `pso make:dispatch-event orders.placed --consume-only`.
- Delivery is at least once: the handler must be idempotent.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [dispatch/events-and-consumers](dispatch/events-and-consumers.md)
- [dispatch/reliability](dispatch/reliability.md)
