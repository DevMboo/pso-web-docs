# Make an event follow a database commit

> Add the Spring transactional outbox, relay rows and handle duplicate delivery safely.

> Status: PARTIALLY implemented — read the limitations.

Suppose an order must be saved and an `orders.created` event published. A direct `save()` followed by `publish()` can lose the event when the process dies between those calls. The Spring outbox inserts the event in the same database transaction as the order; a relay publishes committed rows later.

## Prepare the service

```bash
pso database:enable postgresql --service orders
pso dispatch:install redis --service orders
pso dispatch:outbox --service orders
pso dispatch:validate
```

Configure the PostgreSQL connection and Redis URL through environment variables, and declare `orders.created` as in [the event tutorial](tutorials/event-driven.md). The outbox command creates a Flyway migration for `pso_outbox` and enables `pso.dispatch.outbox.enabled`. Verify the generated service configuration before starting. (A service created before 2026-11-14 may also contain an unrelated `outbox_event` table from an earlier template; it is not the relay's table.)

## Write atomically

Inject the starter's `Outbox` and your repository into a Spring service. Inside one Spring `@Transactional` method, save the order and call `outbox.publish("orders.created", Map.of("orderId", order.id()))`. If event schema validation or the database insert fails, the order transaction rolls back. The [Outbox reference](dispatch/outbox.md) shows the method in context.

The relay claims rows with `FOR UPDATE SKIP LOCKED` and marks one published after broker acknowledgement. A crash between broker acknowledgement and the row update can publish the **same event id** again. Consumers must therefore remain idempotent, even with the outbox. On broker outage, inspect `attempts`, `last_error` and `failed_at`; failed rows remain for investigation. Published rows are eventually removed according to retention.

## Exercise failure recovery

In a non-production test environment, commit an order while the broker is unavailable. The order and outbox row should remain. Restore the broker, then verify the relay publishes the row and the consumer handles it once in business terms. Also roll back a transaction and verify it leaves neither order nor outbox row. Run the service's integration tests for repeatable coverage.

> [!STATUS]
> This outbox is implemented for Spring Boot with JDBC. There is no TypeScript outbox, and delivery remains at least once. See [outbox limits](dispatch/outbox.md).

## What it looks like

The migration the command adds is named `V<n>__pso_outbox.sql` and creates the `pso_outbox` table. Inside the transaction, the order row and the outbox row commit or roll back together:

```java
@Transactional
public OrderResponse create(OrderRequest r) {
    Order saved = orders.save(mapper.toEntity(r));
    outbox.publish("orders.created", Map.of("orderId", saved.getId().toString()));   // rolls back with the order
    return mapper.toResponse(saved);
}
```

During a broker outage the request still succeeds: the row waits (with its attempt count and the error class) and the relay publishes it when the broker returns. The behavior (commit and rollback atomicity, relay, broker failure and recovery, maximum attempts, concurrent relays, retention) is covered by the framework's tests against a real PostgreSQL and Redis; the page [Transactional outbox](dispatch/outbox.md) lists them.

## Check it yourself

```sql
SELECT id, attempts, published_at IS NOT NULL AS published, failed_at IS NOT NULL AS failed, left(coalesce(last_error, ''), 40) AS last_error FROM pso_outbox ORDER BY created_at;
```

The columns come from the generated migration (verified from the command's source): `id`, `created_at`, `attempts`, `next_attempt_at`, `last_error`, `published_at`, `failed_at` (plus the event's own columns). Expected behavior, not captured in a transcript: a freshly committed row has `attempts = 0`, `published = false`; after the relay runs, `published = true`. With the broker down the row stays unpublished and `attempts` grows with the error class in `last_error`; after `max-attempts` the row gets `failed_at` and stays for you to inspect.

## Variations and decisions

| Question | Answer |
|---|---|
| must the event be atomic with my data? | yes: outbox. no: direct publish is simpler ([decision](decisions/outbox-or-direct-publish.md)) |
| the orchestrator needs this | it has no outbox by design ([why](decisions/why-no-typescript-outbox.md)): emit the event from the owning Spring service |
| duplicates | possible after a crash between acknowledgement and marking published; consumers deduplicate on the event id |

## Common mistakes

- Calling `publish` on the broker client directly inside the transaction.
- Expecting the consumer to have run when the HTTP response returns.
- Deleting failed rows to "clean up" before understanding the error.
