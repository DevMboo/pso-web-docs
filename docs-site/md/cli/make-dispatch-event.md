# pso make:dispatch-event

> Declare a DISTRIBUTED PSO Dispatch event and its payload contract. Spring: dispatch/*.events.json and a payload record; orchestrator: src/dispatch/<module>/EVENTS.pso.ts and a schema. --consume-only declares an event another service publishes

Declare a DISTRIBUTED PSO Dispatch event and its payload contract. Spring: dispatch/*.events.json and a payload record; orchestrator: src/dispatch/<module>/EVENTS.pso.ts and a schema. --consume-only declares an event another service publishes.

## Usage

```text
pso make:dispatch-event <event.name> [--service <name>] [--fields "orderId:string,total:number,note?:string"] [--key <field>] [--channel <name>] [--version 1] [--consume-only] [--module <dir>] [--dry-run] [--force] [--json]
```

## Flags

| Flag | What it does |
|---|---|
| `--service` | See the usage line and the task guide for this option. |
| `--fields` | The payload: `name:type` with type string, integer, number or boolean; `name?` is optional. |
| `--key` | A required field that orders the events of one entity. |
| `--channel` | The broker channel; default `<first segment of the name>.events`. |
| `--version` | The contract version; a published version is never edited. |
| `--consume-only` | This project receives the event and never publishes it. |
| `--module` | Orchestrator only: the folder under `src/dispatch/`. |
| `--dry-run` | See the usage line and the task guide for this option. |
| `--force` | See the usage line and the task guide for this option. |
| `--json` | See the usage line and the task guide for this option. |

## Examples

```bash
pso make:dispatch-event orders.placed --service orders --fields "orderId:string,customerId:string,total:number" --key orderId
pso make:dispatch-event orders.placed --service billing --consume-only --fields "orderId:string,customerId:string,total:number"
pso make:dispatch-event orders.placed --service gateway --consume-only --fields "orderId:string"
```

## What it changes

- Declares a DISTRIBUTED event: one that travels through the configured broker (Redis Streams, RabbitMQ, Kafka) to every service that declares it.
- Spring Boot: adds the event with its schema to `src/main/resources/dispatch/<service>.events.json` and writes the payload record `application/event/OrdersPlacedEvent.java` (`events/` in a 0.1 service).
- Orchestrator: adds the event to `src/dispatch/<module>/EVENTS.pso.ts` (created when missing) and writes its schema under `schemas/`.
- `--consume-only` declares an event another service publishes (`publish: false`).

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
updated    src/main/resources/dispatch/orders.events.json
created    src/main/java/com/pso/orders/application/event/OrdersPlacedEvent.java
```

## Verify the result

- `pso dispatch:validate` checks the declaration and `pso dispatch:events` lists it; `pso dispatch:lock` records the contract.
- Declarations are inert until a provider is installed (`pso dispatch:install`) and Dispatch is enabled.

## Failure cases

- `orders.placed version 1 is already declared`: declare the next `--version` for a change.
- `@pso/dispatch is not a dependency of this orchestrator`: run `pso dispatch:install <provider>` first.
- It is not `pso make:event`, which creates a local Spring application event.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [dispatch/events-and-consumers](dispatch/events-and-consumers.md)
- [dispatch/contracts](dispatch/contracts.md)
