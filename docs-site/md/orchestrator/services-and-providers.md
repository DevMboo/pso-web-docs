# Services and providers

> Injectable classes that hold logic belonging to the orchestrator - how to create them, inject them into controllers and into each other, share them between modules, and what should not live in them.

<!-- pso:structure-note -->
> [!NOTE]
> Examples on this page use the 0.1 orchestrator layout (`src/routes/<module>/`, `src/guards/`, `src/contracts/`), which existing
> projects keep. In the official structure of new projects the same files are in `src/modules/<module>/` (`routes/ROUTES.pso.ts`,
> `controllers/`, `services/`, `providers/`, `dto/`) and in `src/.app/` (`guards/`, `middlewares/`, `filters/`, `contracts/`); what the
> page describes works the same way. See [TypeScript orchestrator structure](architecture/typescript-orchestrator.md).

## What a provider is

A **provider** is a class the orchestrator creates for you, once, and hands to whoever asks for it in a constructor. "Service" is the usual name for a provider that holds logic. Anything you list in `providers` of `MODULE.pso.ts` is a provider; nothing is discovered by scanning.

```ts
// users/users.service.ts
import { Injectable } from "@pso/http";
import type { CreateUserDto } from "./dto/create-user.dto";

@Injectable()
export class UsersService {
  findAll() { return [{ id: "1", name: "ada" }]; }
  findById(id: string) { return { id }; }
  create(dto: CreateUserDto) { return { id: "2", ...dto }; }
  remove(id: string) { return { removed: id }; }
}
```

## Why it exists

A controller should read the request and return a result. If it also composes several upstream calls, reshapes responses and applies rules, it becomes impossible to test and to reuse. A provider gives that logic a home with a name, and the container takes care of creating it and passing it where it is needed, so no controller builds its own dependencies.

## When to use it, and when not to

| Put it in a provider | Keep it out |
|---|---|
| composing calls to several upstream services into one public response | domain rules and persistence: they belong to the microservice that owns the data |
| mapping or shaping data for the public API | anything that needs a database of its own (the orchestrator has none) |
| rules the orchestrator itself owns (for example, which fields the public API exposes) | secrets handling: read them through configuration, never hard-code them |
| a [business validator](orchestrator/dto-validation.md) that needs another provider | request parsing: that is the controller's job |

## 1. Create one

```bash
pso make:module orders
pso make:service orders            # creates src/routes/orders/orders.service.ts and registers it
pso make:service orders pricing --export   # a second provider, also exported to modules that import this one
```

Expected output:

```text evidence="expected"
updated  src/routes/orders/MODULE.pso.ts
created  src/routes/orders/orders.service.ts
updated  src/routes/orders/MODULE.pso.ts
created  src/routes/orders/pricing.service.ts
```

The module after both commands (you edit the service files; the module is maintained for you):

```typescript title="src/routes/orders/MODULE.pso.ts"
export default defineModule({
  name: "orders",
  controllers: [],
  providers: [OrdersService, PricingService],
  routes: ordersRoutes,
  imports: [],
  exports: [PricingService]      // only because of --export
});
```

The new file is an empty class. Add `@Injectable()` once it has constructor dependencies, so TypeScript records their types (the container reads that record, which is why the orchestrator is compiled with `emitDecoratorMetadata`).

## 2. Inject it into a controller

```typescript title="orders.controller.ts"
@Controller()
@RestController("/orders")
export class OrdersController {
  constructor(private readonly orders: OrdersService) {}

  @Get("/")
  async list() { return this.orders.list(); }
}
```

The route still needs its entry in `ROUTES.pso.ts` (see [routing](orchestrator/routes.md)). Having a provider exposes nothing.

## 3. Providers that depend on providers

```typescript title="pricing.service.ts"
@Injectable()
export class PricingService {
  total(items: Array<{ priceCents: number; quantity: number }>): number {
    return items.reduce((sum, i) => sum + i.priceCents * i.quantity, 0);
  }
}
```

```typescript title="orders.service.ts"
@Injectable()
export class OrdersService {
  constructor(private readonly pricing: PricingService) {}
  summarize(items: Array<{ priceCents: number; quantity: number }>) { return { total: this.pricing.total(items) }; }
}
```

Both are in `providers`, so the container builds `PricingService` first and passes it to `OrdersService`. They are **singletons**: one instance for the process, so do not keep per-request state in a field.

## 4. Share across modules

Export from the module that owns the provider, import that module from the one that needs it:

```typescript
// billing/MODULE.pso.ts
export default defineModule({ name: "billing", controllers: [], providers: [], routes: billingRoutes, imports: [ordersModule], exports: [] });
```

`BillingController` may now ask for `PricingService` in its constructor. A dependency that is not local, not exported by an imported module and not built in (`Upstream`) stops validation with `PSO_MODULE_006`.

## 5. The built-in `Upstream`

Calling another service is the one dependency you never declare: add `private readonly upstream: Upstream` to a constructor and use `forward(...)`. Unknown services and failures turn into `502`, `503` or `504` problems ([upstream clients](orchestrator/upstream-clients.md)).

## What happens when it starts

1. `pso validate` (and the start-up) check, without running your code, that every constructor dependency resolves (`PSO_MODULE_006`) and that providers do not depend on each other in a circle (`PSO_MODULE_007`, reported as `A -> B -> A`).
2. At start-up the container creates the providers listed in each module and the controllers, in dependency order.
3. Business validators declared on DTOs are providers too, and are resolved at that time. One that is not listed in the module providers stops the start-up with the route and the class name.

A failure at either step means nothing started: the application never half-works.

## Anti-patterns

| Do not | Why | Do instead |
|---|---|---|
| `new PricingService()` inside a controller | the container cannot see it, tests cannot replace it | declare it in the constructor |
| keep the current user or request in a provider field | providers are shared by all requests | pass it as an argument; the verified principal of the request is injected into a controller action with `@CurrentPrincipal()` |
| a provider that stores orders in memory "for now" | the orchestrator is stateless and may run in several copies | the owning microservice stores the data |
| a circular pair (`A` needs `B`, `B` needs `A`) | rejected before start | extract the shared part into a third provider |
| listing a class twice, or in two modules, to "fix" resolution | hides the real ownership | export from one module and import it |

## Troubleshooting

| Message | Meaning |
|---|---|
| `PSO_MODULE_006 Dependency cannot be resolved` | the constructor asks for a class that is not a provider of this module, not exported by an imported module and not built in; add it to `providers` or `imports` |
| `PSO_MODULE_007 Circular provider dependency` | the message shows the cycle; break it |
| `PSO_MODULE_008 Imported module not found` | `imports` names a module that does not exist under `src/routes` |
| a constructor parameter arrives `undefined` | the class is missing `@Injectable()`, or the project is not compiled with `emitDecoratorMetadata` |

## Production considerations

Providers live as long as the process. Release resources (timers, connections) in a graceful shutdown hook of your own, not in constructors. Keep constructors fast and side-effect free: the container creates providers before the listener opens, so slow work delays readiness.

## Providers, hooks and configuration files: what each one is

The orchestrator template has four folders next to `src/routes/`. None of them is scanned: a file takes part in the application only through an explicit declaration or an ordinary import.

| Artifact | Generator | What it is | How it becomes active |
|---|---|---|---|
| Module service | `pso make:service <module> [name]` | a class in the module's folder | listed in `providers` of that `MODULE.pso.ts` (the generator does it) |
| Provider | `pso make:provider <name> --module <module>` | an `@Injectable()` class in `src/providers/`, for logic shared by several modules | listed in `providers` of each module that uses it (`--module` adds it to one) |
| Lifecycle hook | `pso make:hook <name> --module <module>` | a provider in `src/hooks/` that implements `onReady` and/or `onShutdown` | listed in `providers` of a module; a hook nobody lists never runs |
| Configuration | `pso make:config <name>` | a plain module in `src/configs/` that reads its values once | imported where it is needed; the framework neither loads nor injects it |
| `src/services/` | none | a place for plain classes of your own | imported and listed like any other provider |

```bash
pso make:provider clock --module users --service gateway
pso make:hook audit --module users --service gateway
pso make:config storage --service gateway
```

```ts title="src/routes/users/MODULE.pso.ts (after the two commands)"
import { defineModule } from "@pso/core";
import usersRoutes from "./ROUTES.pso";
import { UsersController } from "./users.controller";
import { ClockProvider } from "../../providers/clock.provider";
import { AuditHook } from "../../hooks/audit.hook";

export default defineModule({
  name: "users",
  controllers: [UsersController],
  providers: [ClockProvider, AuditHook],
  routes: usersRoutes,
  imports: [],
  exports: []
});
```

### Lifecycle hooks

```ts title="src/hooks/audit.hook.ts"
import { Injectable, log, type OnReady, type OnShutdown } from "@pso/http";

@Injectable()
export class AuditHook implements OnReady, OnShutdown {
  async onReady(): Promise<void> {
    log("info", "hook ready", { component: "audit" });
  }

  async onShutdown(): Promise<void> {
    log("info", "hook stopped", { component: "audit" });
  }
}
```

| | When | If it throws |
|---|---|---|
| `onReady` | once, after every start-up step succeeded and the listener is bound, just before the application reports ready. Hooks run one at a time, in module order and, inside a module, in the order of `providers` | start-up stops (`PSO_HOOK_FAILED`): everything acquired is released, the hooks that had started get `onShutdown`, and the process never reports ready |
| `onShutdown` | once during shutdown, after the running requests finished (or the grace period ended) and before the framework releases its own resources; reverse order of `onReady` | logged, and the shutdown continues |

A hook's constructor dependencies are injected like any provider's (other providers of the module, `Upstream`, `UpstreamClient`, `Dispatch`). Hooks are for start and stop work (warming a cache, announcing the instance, flushing a buffer): per-request behaviour belongs in a guard, a middleware or a filter. To verify: start the orchestrator and look for `hook ready` and `lifecycle hooks ran` before `orchestrator listening` in the log.

### Configuration files

```ts title="src/configs/storage.config.ts"
export default Object.freeze({
  name: "storage",
  bucket: process.env["STORAGE_BUCKET"] ?? "local",
});
```

```ts
import storageConfig from "../../configs/storage.config";
```

It is application configuration, read when the module is first imported. Framework settings (limits, security, logging, dispatch) are not set there: they live in `pso.config.json` and are listed by `pso config:list`. In a Spring service `pso make:config cache --service catalog` writes a `@Configuration` class, which Spring's component scan finds like any bean of the service.

## Next steps

- [Dependency injection (concept)](concepts/dependency-injection.md) and [in the orchestrator](orchestrator/dependency-injection.md)
- [Module composition](concepts/module-composition.md)
- [Validate request bodies](orchestrator/dto-validation.md) (business validators are providers)
