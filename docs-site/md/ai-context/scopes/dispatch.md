# Dispatch rules

> How assistants must work with PSO Dispatch (events between services).

PSO Dispatch is implemented, **optional and disabled by default**. One event model serves TypeScript (`@pso/dispatch`) and Java (`pso-dispatch-api` + one provider starter); providers are Redis Streams, RabbitMQ and Kafka/Redpanda. Only `pso dispatch:health` does not exist (exit code 2).

Rules:

- Use the CLI to install: `pso dispatch:install <redis|rabbitmq|kafka> --service <name>`, then `pso dispatch:validate`. Never hand-write broker clients, never add `ioredis`, `amqplib`, `kafkajs`, Spring AMQP, Spring Kafka or Spring Data Redis for messaging.
- Declare every event first, with the generator: `pso make:dispatch-event <domain.event> --service <name> --fields "id:string,total:number" [--key id] [--consume-only]` writes the declaration (`EVENTS.pso.ts` + schema in TypeScript, `src/main/resources/dispatch/*.events.json` + a payload record in Java), and `pso make:dispatch-handler <domain.event> --service <name>` writes a declared consumer. A published version is never edited: declare the next `--version`.
- Layered services (0.2.0-rc.2): the Dispatch and event generators write to `events/`, which `pso validate` refuses. After generating, move the classes to `application/event/` and change their `package` line; then `pso validate` must pass. Never leave an `events/` package in a layered service.
- `pso make:event` / `pso make:listener` are NOT Dispatch: they generate a LOCAL Spring application event and `@EventListener` (same process, not durable). Never use them for an event another service must receive.
- Publish only through `Dispatch.publish(type, payload)` (TypeScript, injected) or `PsoDispatch.publish(type, payload)` (Java). Consume with `@DispatchConsumer("event")` listed in `EVENTS.pso.ts`, or `@PsoEventHandler("event")`.
- Delivery is **at-least-once**. Write idempotent handlers. Never promise or describe exactly-once, ordering across partitions, or delayed retries. Retries are in-process with backoff; exhausted and poison messages go to `<channel>.dlq`.
- Dispatch stays `enabled: false` until the user sets it; secrets are environment references. Never print, log or invent broker URLs, passwords or tokens.
- The transactional outbox exists for Spring services with PostgreSQL only (`pso dispatch:outbox`, `Outbox#publish` inside the transaction). There is no TypeScript outbox. The outbox table is `pso_outbox`; an `outbox_event` table found in an older service is a leftover of an earlier template, unused by PSO and no longer generated.
- Not implemented: OpenTelemetry export, consumer lag in Studio, exactly-once, delayed redelivery. Do not generate code that assumes them.
- Use `pso dispatch:inspect`, `dispatch:events`, `dispatch:consumers`, `dispatch:graph` to read the declared setup (no broker is contacted) and `pso dispatch:doctor` for a connection test.
