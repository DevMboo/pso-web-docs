# Connect multiple services

> Register two Spring services, route public requests through the gateway and inspect the topology.

Create a second service only when it owns a separate responsibility or data boundary. In this example, `catalog` owns products and `orders` owns orders; neither service shares the other's database.

```pso-example
pso create workspace shop
cd shop
pso create service catalog
pso create service orders
pso create orchestrator gateway
pso make:resource product --service catalog --route /products --scopes products:read
pso make:resource order --service orders --route /orders --scopes orders:read
pso validate
pso graph
pso routes:list
```

The workspace registers each service with its port and health endpoint. `make:resource` writes each gateway route with the target service in its metadata. At runtime `Upstream.forward` uses that declared service, rather than an arbitrary URL in controller code. Check the generated controller and the [Upstream contract](orchestrator/upstream-clients.md) before changing forwarding behavior.

After setting JWT configuration and installing the generated gateway dependencies, run `pso dev`. `pso services:health` distinguishes a registered service from a live one. Use `pso routes:inspect product` and `pso routes:inspect order` to verify the target, scope and Spring handler for each route.

If `orders` is down, its forwarded route fails while unrelated routes can still work. The gateway maps upstream failures to documented HTTP errors; check `.pso/logs/`, the service health endpoint and [the request lifecycle](orchestrator/request-lifecycle.md). For service-to-service calls originating inside Spring, use [PsoHttpClient](spring/http-client.md) with a named destination and explicit timeouts.

## What you will see

Expected output of the commands above:

```text evidence="expected"
$ pso validate
catalog: 5 resource(s), digest 94045c91b9f4
orders: 5 resource(s), digest 5bc2846a3949
gateway: 9 resource(s), digest cae8b588b014
OK

$ pso services:list
catalog              service       spring-boot  :8081  deps=[]
gateway              orchestrator  node         :3000  deps=[]
orders               service       spring-boot  :8082  deps=[]

$ pso graph
startup order: catalog -> gateway -> orders
gateway routes GET /orders -> orders
gateway routes GET /products -> catalog

$ pso routes:list
GET     /orders                  order.findAll        -> orders         guard:jwt
GET     /products                product.findAll        -> catalog        guard:jwt

$ pso security:policy
default: deny
GET     /orders                  guarded:jwt scopes=[orders:read]  -> orders
GET     /products                guarded:jwt scopes=[products:read]  -> catalog
service catalog: deny by default
service orders: deny by default
```

Two things to notice. Each service got its own port (8081, 8082) automatically. And the start-up order puts the gateway **before** `orders`, because nothing declares that the gateway depends on it: independent services start in name order. The gateway still works (a request to `/orders` before `orders` is up is a `503`), but readiness is reported earlier than the system is truly usable. Declare the dependency when it matters: edit `dependsOn` of `gateway` in `pso.workspace.json` (or register with `--depends-on`), then `pso graph` shows `orders` first.

## Failure behavior to test

1. Start everything, then stop `orders`. A request to `/orders` answers `503 PSO_UPSTREAM_UNAVAILABLE`; `/products` still works.
2. `pso services:health` exits 1 and names `orders` as `DOWN`.
3. Start `orders` again: the next request succeeds with no restart of the gateway.

## Variations

| You want | Do |
|---|---|
| `orders` to call `catalog` itself | `PsoHttpClient` with a declared destination ([service-to-service](microservices/service-to-service.md)) |
| `orders` to be told when a product changes | an event ([HTTP or Dispatch?](decisions/http-or-dispatch.md)) |
| separate databases | `pso create service orders --db postgres` for each ([persistence](microservices/persistence.md)) |

## Common mistakes

- Sharing a database between `catalog` and `orders` "to save time".
- Forgetting `dependsOn` and then reading start-up order as a dependency order.
- Putting both resources' routes in one module: keep one module per resource.
