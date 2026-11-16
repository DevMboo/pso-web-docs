# Build your first application

> Create a workspace, a Java service and a TypeScript gateway, then trace a request through both.

A PSO workspace contains one public orchestrator and any number of services. The orchestrator owns the public route; a Spring service owns the implementation. This walkthrough creates both and checks the generated declarations before starting anything.

You will finish with a **declared, secured `GET /products` route** and a generated Spring handler that returns `[]`. That is a working transport and security path, not a persistence layer. The [CRUD tutorial](tutorials/crud-api.md) builds the database-backed version next.

| Checkpoint | What you should know before moving on |
|---|---|
| After generation | `pso.workspace.json` names both processes; the route registry names the public URL and target service |
| After `pso validate` | PSO accepts the file structure and declarations; no process has been contacted |
| After `pso dev` and `services:health` | Both health endpoints answer; that alone does not prove authorization or the product response |
| After the HTTP request | A correctly scoped token reaches Spring and receives the handler's `[]` response |

## Prerequisites

Install the requirements in [Requirements](getting-started/requirements.md), including Node.js, Java 21 and the project-local Maven artifacts (`pso java:install`). Run these commands from a directory where you want a new `shop` folder.

The CLI and generated gateway need Node 24. Install the CLI from the framework checkout, run `pso java:install` there, and return to the parent directory where `shop/` will be created. For the runtime step you will also need gateway dependencies and a development JWT verifier shared with Spring; [Running the environment](getting-started/running.md) gives the exact setup.

```pso-example
pso create workspace shop
cd shop
pso create service catalog
pso create orchestrator gateway
pso make:resource product --service catalog --route /products --scopes products:read
pso validate
pso routes:list
```

`make:resource` creates matching gateway declarations, a Spring controller and DTO, a shared contract and tests. Inspect `gateway/src/routes/product/MODULE.pso.ts` and `ROUTES.pso.ts`: the module lists its controller, while the route registry is the authority for public exposure. The `@Get` decorator on a controller method alone does not open a URL. `pso validate` catches disagreement without executing application code.

The generated files have different jobs:

```tree title="The request path created by make:resource"
shop/
  pso.workspace.json  # generated: gateway and catalog names, ports and roles
  gateway/
    src/
      routes/
        product/
          MODULE.pso.ts  # generated: composes the product controller
          ROUTES.pso.ts  # generated: exposes GET /products with products:read
          product.controller.ts  # generated: forwards the request to catalog
      contracts/
        product.contract.json  # generated: gateway copy of the contract
  catalog/
    src/
      main/
        java/com/pso/catalog/routes/ProductController.java  # generated: returns an empty list
        resources/contracts/product.contract.json  # generated: service copy of the contract
```

Read the path from top to bottom: the workspace locates `catalog`; the route decides whether the URL exists and who may call it; the gateway controller forwards; Spring checks the token again and produces the response. The two contract files are mirrors, so changing just one creates drift that `pso validate` reports.

The generated resource uses a guarded route. Configure the JWT issuer, audience and public key for **both** processes as described in [Running the environment](getting-started/running.md), then start from the workspace root:

```bash
pso dev
```

In another terminal, `pso services:health` checks the processes. `pso routes:inspect product` shows the route, target service and access policy. An unauthenticated `GET /products` returns `401`; send a valid bearer token with the `products:read` scope to exercise the Spring endpoint. The generated list handler initially returns an empty list; replace that application stub with your own data access when building a real service.

```bash
curl -i http://127.0.0.1:3000/products
curl -i http://127.0.0.1:3000/products -H "authorization: Bearer $TOKEN"
```

The first request is a useful control: it must stop at the gateway with `401` before Spring is called. For the second, create `$TOKEN` with the development issuer, audience, public key and `products:read` scope configured in [Running the environment](getting-started/running.md). It should return `200` and `[]` when `catalog` is running. If it returns `403`, inspect the token scope; if it returns `503`, inspect service health. The exact error bodies are in [First HTTP endpoint](getting-started/first-endpoint.md).

## If it fails

If Maven cannot resolve `com.pso` artifacts, run `pso java:install` in the framework checkout. If the orchestrator cannot find packages, install its dependencies in `gateway`. If a request returns `404`, compare the public path in `ROUTES.pso.ts` with `pso routes:list`. See [first-run troubleshooting](getting-started/troubleshooting.md).

Next: [Build a CRUD API](tutorials/crud-api.md), or read [routing](orchestrator/routes.md) to understand the three declarations in detail.

## What each command printed

Expected output of a real run of exactly these commands (file lists abridged):

```text evidence="expected"
$ pso make:resource product --service catalog --route /products --scopes products:read
created    gateway/src/contracts/product.contract.json
created    catalog/src/main/java/com/pso/catalog/contracts/ProductDto.java
created    catalog/src/main/java/com/pso/catalog/routes/ProductController.java
created    catalog/src/main/resources/contracts/product.contract.json
created    catalog/src/test/java/com/pso/catalog/routes/ProductControllerTests.java
created    docs/api/product.md
created    gateway/src/routes/product/MODULE.pso.ts
created    gateway/src/routes/product/ROUTES.pso.ts
created    gateway/src/routes/product/product.controller.ts

$ pso validate
gateway: 5 resource(s), digest 1ce8bc49aa9c
catalog: 5 resource(s), digest 94045c91b9f4
OK

$ pso routes:list
GET     /products                product.findAll        -> catalog        guard:jwt

$ pso routes:inspect product
GET /products  ->  catalog:8081
access:   guard jwt, scopes products:read
defined:  gateway/src/routes/product/ROUTES.pso.ts
contract: present
handlers: catalog/src/main/java/com/pso/catalog/routes/ProductController.java
```

`routes:inspect` accepts a module name (`product`), a route id (`product.findAll`) or an exact path (`/products`); it does not accept a method and a space (`"GET /products"`).

## What the requests answer

With the gateway running and JWT configured, and the `catalog` service **not** started yet, the answers are (verified; see [First endpoint](getting-started/first-endpoint.md) for the full bodies and logs):

| Request | Answer |
|---|---|
| `GET /products`, no token | `401 PSO_AUTH_UNAUTHORIZED` |
| token without `products:read` | `403 PSO_AUTH_FORBIDDEN` |
| token with the scope, service down | `503 PSO_UPSTREAM_UNAVAILABLE` |
| after starting `catalog`, same request | the service's answer (`200`; the generated list is empty: `[]`) |

The `503` is useful: it proves authentication, authorization and routing all worked and that only the service is missing.

## Variations

- Add a second resource and watch `pso routes:list` grow ([multiple services](tutorials/multiple-services.md)).
- Make the endpoint public instead of guarded to see the difference in `pso security:policy`.
- Replace the generated `List.of()` with real data ([CRUD tutorial](tutorials/crud-api.md)).

## Recovering from mistakes

| Mistake | What you see | Fix |
|---|---|---|
| used Git Bash for `--route /products` | `error: Invalid route "C:/Program Files/Git/products"` | PowerShell, or `MSYS_NO_PATHCONV=1` |
| edited only one contract copy | `PSO_CONTRACT_DRIFT` from `pso validate` | update both, or `pso make:resource ... --force` |
| forgot JWT settings | every guarded request `401`; log `authentication failed reason=...` | [Running the environment](getting-started/running.md) |
