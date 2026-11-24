# Orchestrator conventions

> Node.js + TypeScript orchestrator layout, API, dependency injection and guards.

## Layout (src/)

| Kind | Directory | File |
|---|---|---|
| module | `src/modules/<module>/` | `MODULE.pso.ts` |
| route-registry | `src/modules/<module>/routes/` | `ROUTES.pso.ts` |
| controller | `src/modules/<module>/controllers/` | `*.controller.ts` |
| service | `src/modules/<module>/services/`, `src/shared/`, `src/shared/.../` | `*.service.ts` |
| provider | `src/modules/<module>/providers/` | `*.provider.ts` |
| dto | `src/modules/<module>/dto/`, `src/modules/<module>/dto/request/`, `src/modules/<module>/dto/response/`, `src/shared/`, `src/shared/.../` | `*.dto.ts` |
| mapper | `src/modules/<module>/mappers/` | `*.mapper.ts` |
| type-contract | `src/modules/<module>/contracts/`, `src/.app/contracts/` | `*.contract.ts` |
| contract | `src/.app/contracts/` | `*.contract.json` |
| guard | `src/.app/guards/`, `src/modules/<module>/guards/` | `*.guard.ts` |
| middleware | `src/.app/middlewares/`, `src/modules/<module>/middlewares/` | `*.middleware.ts` |
| filter | `src/.app/filters/`, `src/modules/<module>/filters/` | `*.filter.ts` |
| interceptor | `src/.app/interceptors/`, `src/modules/<module>/interceptors/` | `*.interceptor.ts` |
| decorator | `src/.app/decorators/` | `*.decorator.ts` |
| config | `src/.app/configs/` | `*.config.ts` |
| hook | `src/modules/<module>/hooks/`, `src/.app/hooks/` | `*.hook.ts` |
| dispatch | `src/dispatch/`, `src/dispatch/.../` | `*.dispatch.ts` |

Mandatory for new orchestrators (`pso create orchestrator`). `src/main.ts` only calls `bootstrap()`; `src/shared/` holds code of no single module. A 0.1 orchestrator (`src/routes/<module>/`) keeps its layout: see the structure migration guide.

- Entry point `src/main.ts` calls `bootstrap({ appDir: __dirname })` from `@pso/http`. Do not put routing logic there.
- The project is **compiled** with `tsc` (CommonJS, `experimentalDecorators`, `emitDecoratorMetadata`) and runs from `dist/`. Use `npm run build`, `npm run dev` or `pso dev`.
- Relative imports in module files are extensionless (`./users.service`).

## API (from `@pso/http` and `@pso/core`)

- `Controller` — Class decorator. Marks a class as a controller. It does NOT expose anything by itself.
- `RestController` — Class decorator. Sets the path prefix of every action: @RestController("/users").
- `Get` — Method decorator for HTTP GET. The path is relative to the controller prefix. The route is exposed only if ROUTES.pso.ts declares it.
- `Post` — Method decorator for HTTP POST (same rules as Get).
- `Put` — Method decorator for HTTP PUT (same rules as Get).
- `Patch` — Method decorator for HTTP PATCH (same rules as Get).
- `Delete` — Method decorator for HTTP DELETE (same rules as Get). A decorated Delete that is absent from ROUTES.pso.ts answers 404.
- `Param` — Parameter decorator: @Param("id") id: string reads a path parameter as a string. A second argument such as { type: "integer", min: 1 } converts and checks it; a violation is a 400 problem named path.id.
- `Query` — Parameter decorator: @Query("page") reads a query-string value as a string. A second argument such as { type: "integer", min: 1, default: 1 } converts and checks it (strictly, no coercion of "12abc"); a violation is a 400 problem named query.page.
- `Body` — Parameter decorator: @Body() parses the JSON body (size-limited; 400/413/415 on problems) and performs no schema validation. @Body(MyDto) additionally validates it against a @Dto class and calls the action only when it is valid (400 PSO_VALIDATION_FAILED with field errors otherwise).
- `Dto` — Class decorator. Marks a class as a validation contract usable in @Body(MyDto), @Nested and @ArrayOf. @Dto({ unknown: "strip" }) drops undeclared properties; the default rejects them.
- `Required` — Field decorator. The field must be present (this is the default; use it to say so explicitly).
- `Optional` — Field decorator. The field may be absent. TypeScript's ? is erased and is never read.
- `Nullable` — Field decorator. An explicit JSON null is accepted.
- `StringField` — Field type decorator: a JSON string (no coercion).
- `IntegerField` — Field type decorator: a JSON number that is a safe integer (no coercion from strings).
- `NumberField` — Field type decorator: a finite JSON number.
- `BooleanField` — Field type decorator: true or false.
- `Email` — Field type decorator: a string that is a valid e-mail address (at most 254 characters).
- `IsoDate` — Field type decorator: an ISO 8601 date or date-time with a zone; the value stays a string.
- `OneOf` — Field decorator: the value must be one of the listed strings or numbers: @OneOf("admin", "user").
- `Nested` — Field decorator: the value is an object validated by another @Dto class (up to 8 levels). Pass () => Dto for mutually referencing classes.
- `ArrayOf` — Field decorator: an array whose items are "string", "integer", "number", "boolean" or a @Dto class (up to 1000 items unless @MaxItems says otherwise, never more than 10000).
- `MinLength` — Field constraint: minimum string length in characters.
- `MaxLength` — Field constraint: maximum string length in characters.
- `Min` — Field constraint: minimum numeric value (inclusive).
- `Max` — Field constraint: maximum numeric value (inclusive).
- `MinItems` — Field constraint: minimum array length.
- `MaxItems` — Field constraint: maximum array length.
- `Pattern` — Field constraint: the string must match the regular expression (no g or y flag).
- `Message` — Field decorator: replaces the validation messages of that field with a constant of yours. Never put request data in it.
- `Validate` — Field decorator: a business validator class (a provider with validate(value, ctx) that may be async and use injected services). It must be listed in the module providers and runs only after the structure is valid.
- `Req` — Parameter decorator: injects the Node IncomingMessage.
- `Res` — Parameter decorator: injects the Node ServerResponse (used with Upstream.forward).
- `Injectable` — Class decorator. Lets the DI container construct a provider that has constructor dependencies.
- `Public` — Class or method decorator. Declares the action public. A route that is public AND guarded is rejected (PSO_ROUTE_007).
- `UseGuards` — Class or method decorator. Adds guard names that must pass. Requirements add up; a route declaration can never weaken them.
- `Scopes` — Class or method decorator. Adds scopes the guard must find in the token.
- `defineRoutes` — Declares the authoritative route registry of a module (ROUTES.pso.ts). Only literal, statically analyzable values are accepted.
- `defineModule` — (from @pso/core) Declares a module in MODULE.pso.ts: name, controllers, providers, routes, imports, exports.
- `bootstrap` — Starts the orchestrator: discovers modules in the compiled app directory, validates, resolves dependencies, registers only validated routes.
- `Upstream` — Built-in provider. upstream.forward(req, res, "service") proxies the request; the service must equal the route's metadata.service.
- `UpstreamClient` — Built-in provider for COMPOSING a response from several registered services: services.get<T>("service", "/path/:id", { params, query, credentials, timeoutMs }). The service must be listed in the route's metadata.services (or be its metadata.service). No URL parameter, no redirects, one attempt.
- `UpstreamCallError` — Thrown by UpstreamClient: { service, kind, status?, problem? }. e.is(404) tests the upstream status. Unhandled it becomes a stable problem (404/409 kept, other 4xx 502 PSO_UPSTREAM_REJECTED, 5xx 502) that never carries the upstream body.
- `HttpError` — Throw new HttpError(status, message) from an action to answer with a specific status.

## Rules

- Constructor parameter types are the dependency contract. Classes with dependencies need `@Injectable()`. A dependency must be a provider of the module, exported by an imported module, or `Upstream`.
- In the official structure, application-wide guards live in `src/.app/guards/<name>.guard.ts`; module-local guards live in `src/modules/<module>/guards/`. The `src/guards/` and `src/routes/<module>/guards/` paths belong to 0.1 projects. A guard is `Guard = (req, route) => true | false | { allow: false, status: 401 | 403 }`. Its name is the file name without `.guard.ts`.
- Middleware runs after the guards; filters handle thrown errors and return `true` when handled.
- `@Body()` parses JSON with limits and does **not** validate fields. `@Body(MyDto)` validates against a `@Dto` class (see the add-dto task); otherwise validate in the action or in the Spring service.
- HTTP limits are configuration, not code: `http.limits.*` / `MAX_BODY_BYTES`, `MAX_UPSTREAM_RESPONSE_BYTES`, `UPSTREAM_TIMEOUT_MS`, `MAX_CONCURRENT_REQUESTS` ... (one precedence: variable > `PSO_CFG_*` > `pso.config.<env>.json` > `pso.config.json` > defaults). Oversized bodies are `413`, upstream failures `502/503/504` problems; do not buffer bodies yourself.
- Domain logic does not belong in the orchestrator; forward to the owning service with `this.upstream.forward(req, res, "service")`, and set `metadata: { service: "service" }` on the route.
- To build one response from several services, inject `UpstreamClient` and declare `metadata: { services: ["a", "b"] }` on the route (`pso make:route ... --compose a,b`). It calls registered services by NAME only (no URL), applies each service's prefix, propagates the correlation id, sends the caller's token only with `credentials: "caller"`, has a deadline (`timeoutMs`) and a size limit, and throws `UpstreamCallError`. A service the route did not declare is refused. It makes one attempt: no retries, no transactions across services.
- In the official structure, `pso make:provider|make:hook <name> --module <module>` writes `@Injectable()` classes in `src/modules/<module>/providers/` or `hooks/` and registers them in that module's `MODULE.pso.ts`. A hook implements `onReady()` and/or `onShutdown()`. Application-wide configuration is a plain importable module in `src/.app/configs/`. The top-level `src/providers/`, `src/hooks/` and `src/configs/` paths belong to 0.1 projects. Nothing runs merely because a file exists.
- An unexpected error always answers the sanitized problem `PSO_INTERNAL_ERROR`; never return or log exception messages yourself. The log has the error type and its origin; message and stack only with `logging.errorDetails` (development).
- Optional typing: `pso types` writes `src/pso-types.d.ts` so the compiler checks service and guard names. String declarations stay valid without it.

## Request pipeline (in order)

rate limit (per address; then per principal and route policy after the guards), route match (undeclared = 404), guards (401/403), middleware, parameter and body binding (400/413/415), action, response (201 for POST, 200 otherwise), filters on errors.
