# Publish and consume an event

> Publish an event from an orchestrator action, consume it with a registered consumer over Redis Streams, and watch acknowledgement, retry and dead-lettering happen - with a failing handler on purpose.

Use Dispatch when the publisher can finish its request before another service completes its work. A broker acknowledgement means the event was accepted by the transport; it does not mean a consumer completed its handler. Use synchronous HTTP when the caller needs the answer ([HTTP or Dispatch?](decisions/http-or-dispatch.md)).

This tutorial builds one small flow in an orchestrator: `POST /orders` **publishes** `orders.placed`, a **consumer** handles it and publishes `orders.confirmed`. Then it makes the handler fail to show retries and the dead-letter channel. It was run end to end against a real Redis 7 (`docs-event-driven.test.ts`); the output marked *Verified* comes from that run. The orchestrator is both producer and consumer here because it is the quickest real consumer; a Spring service uses the same envelope and rules (see the end).

## Prerequisites

- A workspace with an orchestrator named `gw`, signing keys (`pso security:keys --generate`) and a token helper: steps 1, 3 and 12 of [Build a CRUD API](tutorials/crud-api.md) create exactly these, including `dev-token.mjs`. The `.env` below uses the same issuer and audience as that tutorial.
- Docker, for Redis. Node 24 or newer.

## 1. Install the provider

```bash
pso dispatch:install redis --service gw
```

```text evidence="verified"
package.json: + @pso/dispatch
package.json: + ioredis
pso.config.json: dispatch.provider = redis (enabled stays false; secrets are ${env:...} references)
src/dispatch/: modules go here, each with an EVENTS.pso.ts
infra/dispatch/redis.compose.yml: a local development broker (not started by PSO)
Next: declare your events, then set pso.dispatch.enabled=true (and the broker URL) to activate. See /docs/dispatch/overview.
```

Dispatch stays **disabled** until you enable it, and PSO never starts the broker. Run `npm install` in `gw/` to fetch the two new dependencies.

## 2. The producer: an action that publishes

A module, a controller and a guarded route, with the generators that keep the three consistent ([routes](orchestrator/routes.md)):

```bash
pso make:module orders --service gw
pso make:controller orders --service gw
pso make:route orders place --method POST --path / --guard jwt --scopes orders:write --service gw
```

Replace the generated action body so it publishes. `Dispatch` is built in like `Upstream`: inject it by constructor type, without listing it in `providers`.

```typescript title="gw/src/modules/orders/controllers/orders.controller.ts"
import { Controller, RestController, Post, Body, HttpError } from "@pso/http";
import { Dispatch } from "@pso/dispatch";

@Controller()
@RestController("/orders")
export class OrdersController {
  constructor(private readonly dispatch: Dispatch) {}

  @Post("/")
  async place(@Body() body: { orderId?: string; fail?: boolean }) {
    if (typeof body?.orderId !== "string" || body.orderId === "") throw new HttpError(400, "orderId is required");
    const result = await this.dispatch.publish("orders.placed", { orderId: body.orderId, ...(body.fail === true ? { fail: true } : {}) });
    return { accepted: true, eventId: result.id };
  }
}
```

`publish` resolves when Redis acknowledged the write (`XADD`). The action answers as soon as that happens; it never waits for the consumer.

## 3. The contract and the consumer

Create `gw/src/dispatch/orders/` with three files. The contract declares both events; the schema is the payload contract of `orders.placed`; the consumer is registered in `consumers`, and a class that is not listed there is never started (`PSO_DISPATCH_008`).

```tree title="What you add under gw/src/dispatch/"
gw/src/dispatch/orders/
  EVENTS.pso.ts  # manual: declares the events and lists the consumers
  schemas/order-placed.schema.ts  # manual: the payload contract of orders.placed
  orders.consumer.ts  # manual: the handler of orders.placed
```

```typescript title="gw/src/dispatch/orders/EVENTS.pso.ts"
import { defineEvents } from "@pso/dispatch";
import { OrderPlacedConsumer } from "./orders.consumer";

export default defineEvents({
  events: [
    { name: "orders.placed", version: 1, channel: "orders.placed", schema: "schemas/order-placed.schema.ts", key: "orderId" },
    { name: "orders.confirmed", version: 1, channel: "orders.confirmed", key: "orderId" },
  ],
  consumers: [OrderPlacedConsumer],
});
```

```typescript title="gw/src/dispatch/orders/schemas/order-placed.schema.ts"
export default { type: "object", required: ["orderId"], properties: { orderId: { type: "string" }, fail: { type: "boolean" } } };
```

```typescript title="gw/src/dispatch/orders/orders.consumer.ts"
import { Dispatch, DispatchConsumer } from "@pso/dispatch";

@DispatchConsumer("orders.placed")
export class OrderPlacedConsumer {
  constructor(private readonly dispatch: Dispatch) {}

  async handle(payload: { orderId: string; fail?: boolean }) {
    if (payload.fail) throw new Error("payment provider unavailable");
    await this.dispatch.publish("orders.confirmed", { orderId: payload.orderId });
  }
}
```

`key: "orderId"` keeps events of one order together (ordering is per stream and the key identifies the entity). The `fail` flag exists only so that you can make the handler fail on demand.

## 4. Validate and lock the contract

```bash
pso dispatch:validate
pso dispatch:lock
pso validate
```

```text evidence="verified"
INFO    PSO_DISPATCH_UNLOCKED gw (src/dispatch/orders/EVENTS.pso.ts)
        orders.placed v1 is not locked yet
        fix: Run `pso dispatch:lock` to record its contract
```

The first command prints that `INFO` line (it does not fail validation) followed by `valid. Static validation only: event delivery is verified by running the service against a broker.` The lock records the contract in `pso.dispatch.lock.json`; commit it, because from now on a changed schema under the same name and version is rejected ([checking Dispatch](dispatch/diagnostics.md)).

## 5. Start Redis and the orchestrator

```bash
docker compose -f infra/dispatch/redis.compose.yml up -d
```

Add to `gw/.env` (the key file name is the one `pso security:keys` printed):

```ini title="gw/.env"
PSO_JWT_ISSUER=https://shop.local
PSO_JWT_AUDIENCE=shop-api
PSO_JWT_PUBLIC_KEY_FILE=../keys/<kid>.public.pem
PSO_JWT_ALGORITHMS=ES256
PSO_WORKSPACE=../pso.workspace.json
PSO_DISPATCH_ENABLED=true
REDIS_URL=redis://127.0.0.1:6379
```

```bash
cd gw
npm run dev
```

Check the connection without starting anything else: `pso dispatch:doctor` answers `UP` when Redis answers ([what the answers mean](dispatch/diagnostics.md)).

## 6. Publish an event

```bash
TOKEN=$(node dev-token.mjs "orders:write")
curl -i -X POST http://127.0.0.1:3000/orders -H "authorization: Bearer $TOKEN" -H "content-type: application/json" -d '{"orderId":"o-100"}'
```

```json evidence="verified"
{"accepted":true,"eventId":"36330c52-d417-42b7-80c0-dc4c691ed687"}
```

The status is `201` and the body carries the id of the published event (yours differs). What happened, in order:

1. the orchestrator verified the token and the `orders:write` scope, and ran `place`;
2. `Dispatch.publish` wrote the event, wrapped in the standard envelope, to the Redis stream `orders.placed`;
3. the consumer group `gw` read it, called `handle`, which published `orders.confirmed`;
4. only then the message was **acknowledged**.

Look at what the consumer produced:

```bash
docker compose -f infra/dispatch/redis.compose.yml exec redis redis-cli XRANGE orders.confirmed - +
```

```json evidence="verified"
{"envelopeVersion":1,"id":"9fc1fc51-108a-4997-a9ae-7f31d808a00d","type":"orders.confirmed","version":1,"source":"gw","occurredAt":"2026-11-08T20:06:02.828Z","correlationId":"59e546d6-22be-4823-a8d4-4145cbeb83e8","contentType":"application/json","payload":{"orderId":"o-100"}}
```

This is the value of the `envelope` field of the stream entry. And the consumer group has nothing left to do:

```bash
docker compose -f infra/dispatch/redis.compose.yml exec redis redis-cli XINFO GROUPS orders.placed
```

```json evidence="verified"
{"name":"gw","consumers":1,"pending":0,"last-delivered-id":"1794168362822-0","entries-read":1,"lag":0}
```

(Shown as JSON for readability; `redis-cli` prints the same pairs as a list.) `pending 0` and `lag 0` mean the message was acknowledged: the consumer finished before the broker was told so.

An invalid request never reaches the broker: `{}` answers `400` (`orderId is required`) and the stream `orders.placed` still has one entry (verified by the test).

## 7. Make the handler fail

```bash
curl -s -X POST http://127.0.0.1:3000/orders -H "authorization: Bearer $TOKEN" -H "content-type: application/json" -d '{"orderId":"o-fail","fail":true}'
```

The publish succeeds (`201`) because publishing and handling are independent. The orchestrator terminal shows the handler failing and being retried in-process while the message stays **unacknowledged**:

```text evidence="verified"
2026-11-08 20:06:04.348 WARN  consumer failed  event=orders.placed id=ad640284-549c-4c4d-b583-f94c33855f7f attempt=1 error=Error
2026-11-08 20:06:04.558 WARN  consumer failed  event=orders.placed id=ad640284-549c-4c4d-b583-f94c33855f7f attempt=2 error=Error
2026-11-08 20:06:04.962 WARN  consumer failed  event=orders.placed id=ad640284-549c-4c4d-b583-f94c33855f7f attempt=3 error=Error
```

(The run used `PSO_CFG_dispatch__retry__initialDelay=200ms` and `maxDelay=400ms` to keep the gaps short; the defaults are 3 attempts, 1 s initial delay, 30 s maximum.) The log names the event type, the id and the attempt; the error is the class, not the message, because messages can contain secrets.

After the third attempt the original message is moved to `orders.placed.dlq`, with headers that say why, and only then acknowledged:

```bash
docker compose -f infra/dispatch/redis.compose.yml exec redis redis-cli XRANGE orders.placed.dlq - +
```

The entry's fields include `h:x-pso-reason` = `handler-failed:Error` and `h:x-pso-attempts` = `3` (verified). `orders.confirmed` still has one entry: a failed handler publishes nothing.

## What you should take from it

| Question | Answer, from the run above |
|---|---|
| when does `publish` resolve? | when Redis accepted the write, not when the consumer finished |
| when is a message acknowledged? | after the handler succeeded, or after it was moved to the dead-letter channel |
| what if the handler keeps failing? | retries with backoff, then `<channel>.dlq`; nothing is lost and nothing is replayed automatically |
| what if the process dies mid-handler? | the message is not acknowledged and is delivered again: delivery is **at least once**, so make handlers idempotent (store the event id, or enforce a unique business key) |
| what does `pso dispatch:validate` prove? | the declarations are consistent; it does not touch the broker |

## In a Spring service

The same flow uses `PsoDispatch` and an `@PsoEventHandler("orders.created")` method with `ConsumeContext`, and the events are declared in `src/main/resources/dispatch/orders.events.json`. This part was not run for this tutorial: see [Events and consumers](dispatch/events-and-consumers.md) and [Dispatch and Spring](spring/dispatch.md). To avoid losing an event between a database commit and the publish, use the [transactional outbox](tutorials/transactional-outbox.md).

## Variations

| You want | Do |
|---|---|
| another broker | `pso dispatch:remove --service gw`, then `pso dispatch:install rabbitmq` or `kafka` ([choosing](decisions/redis-rabbitmq-or-kafka.md)); only connectivity was checked for those two |
| durable deduplication | the Redis idempotency store, or a unique constraint in your handler |
| to see the full set of counters and the poison-message case | [the walkthrough](dispatch/walkthrough.md) |

## Common mistakes

- Assuming a successful publish means the work was done.
- No monitoring of `.dlq` channels.
- A handler with side effects that is not idempotent.
- Forgetting to list the consumer in `consumers`: it is never started.
