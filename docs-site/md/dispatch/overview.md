# Dispatch architecture

> One event contract and one API for TypeScript and Java, with independent Redis Streams, RabbitMQ and Kafka adapters. Optional and disabled by default.

PSO Dispatch moves events between services. The event model is the same in TypeScript (`@pso/dispatch`) and Java (`pso-dispatch-api` plus a provider starter); only the transport differs per provider, and a service uses exactly one.

> [!STATUS]
> Implemented and verified against **real** Redis, RabbitMQ and Kafka brokers (TypeScript and Java adapters, TypeScript to Java on all three). The delivery guarantee is **at-least-once**. Exactly-once is not provided and not claimed. See [Verification and limits](dispatch/local-and-production.md).

Dispatch is also the transport of [PSO Channels](orchestrator/channels.md): a service publishes the `channels.publish` event and the orchestrator delivers it to browsers. Channels never replaces Dispatch and is not a broker.

## The pieces

| Piece | What it is |
|---|---|
| Envelope | The one wire format: `envelopeVersion`, `id`, `type`, `version`, `source`, `occurredAt`, `correlationId`, `causationId`, `trace`, `contentType`, `payload`. Byte-identical canonical JSON in both languages ([Contracts](dispatch/contracts.md)) |
| Event declaration | An explicit, versioned contract per event: `EVENTS.pso.ts` in TypeScript, `dispatch/*.events.json` in Java |
| API | `Dispatch.publish(type, payload)` in TypeScript, `PsoDispatch.publish(type, payload)` in Java. No broker type appears in application code |
| Consumers | `@DispatchConsumer("event")` classes (TypeScript), `@PsoEventHandler("event")` methods (Java) |
| Adapter | One per provider, behind a stable contract with capability metadata. Core packages contain no broker client |
| Runtime | Validation, idempotency, retry with backoff, dead-letter, acknowledgement only after the handler succeeded |
| Outbox | Optional, Java only: the event is stored in your database transaction and relayed afterwards ([Outbox](dispatch/outbox.md)) |

## Optional by design

A new project has dispatch **configured but disabled**: `pso.dispatch.enabled=false`, `provider: none`, no adapter dependency, no broker connection, no topology. You opt in with `pso dispatch:install <provider>` and then set `enabled: true`. See [Why dispatch is optional](decisions/why-dispatch-optional.md) and [Installation](dispatch/installation.md).

## What is not provided

- Exactly-once processing. Publishing to a broker and committing a database transaction are not atomic; the outbox narrows the gap and consumers deduplicate on the event id.
- Delayed redelivery. Retries happen in the consumer process while the message stays unacknowledged.
- A TypeScript outbox, OpenTelemetry export and consumer-lag display in Studio. See [Observability and security](dispatch/security-and-observability.md).
- `pso dispatch:health` (exits with code 2); use `pso dispatch:doctor`.

## See it run

[Walkthrough: publish, consume, retry, dead-letter](dispatch/walkthrough.md) is a real run over Redis with the commands, the broker contents, the logs and the metrics. Read it before deciding whether you need events at all ([HTTP or Dispatch?](decisions/http-or-dispatch.md)), and use [Redis Streams, RabbitMQ or Kafka?](decisions/redis-rabbitmq-or-kafka.md) to choose a provider.

## Common mistakes

- Enabling dispatch "just in case": it is optional on purpose.
- Assuming a successful `publish` means the consumer finished (it means the transport accepted it).
- Non-idempotent handlers under at-least-once delivery.
- Never looking at the `.dlq` channels.

## Next steps

- [Walkthrough](dispatch/walkthrough.md)
- [Installation](dispatch/installation.md)
- [Reliability](dispatch/reliability.md)
