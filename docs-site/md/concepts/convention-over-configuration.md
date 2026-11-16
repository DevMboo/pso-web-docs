# Convention over configuration

> The directory and naming rules PSO enforces for each runtime, why they are enforced and not suggested, what the diagnostics look like, and where explicit configuration begins.

Conventions are enforced, not suggested: a file with a known suffix in the wrong place is `PSO_MISPLACED_RESOURCE`, a badly named file is `PSO_BAD_NAME`.

## Why enforce

If a convention is only advice, tools must handle every variation, and each variation is a place for the structure to stop meaning anything. If it is enforced, every PSO project looks the same, `pso validate` can answer "is this wired correctly?" without running anything, and a new developer finds `guards` in `guards`.

The cost is real: you cannot invent your own layout. The trade is described in [Why directory conventions](decisions/why-directory-conventions.md).

## Node.js orchestrator

HTTP code lives in modules, `src/routes/<module>/`. Every module needs both `MODULE.pso.ts` and `ROUTES.pso.ts`. Names are `kebab-case`.

| Kind | Directory | File pattern |
|---|---|---|
| module | routes/ | `MODULE.pso.ts` |
| route-registry | routes/ | `ROUTES.pso.ts` |
| controller | routes/ | `*.controller.ts` |
| dto | routes/ | `*.dto.ts` |
| middleware | middlewares/ (or routes/) | `*.middleware.ts` |
| filter | filters/ (or routes/) | `*.filter.ts` |
| guard | guards/ (or routes/) | `*.guard.ts` |
| service | services/ (or routes/) | `*.service.ts` |
| dispatch | dispatch/ | `*.dispatch.ts` |
| config | configs/ | `*.config.ts` |
| provider | providers/ | `*.provider.ts` |
| hook | hooks/ | `*.hook.ts` |
| contract | contracts/ | `*.contract.json` |

A generated orchestrator already has the right directories (Expected output of `pso create orchestrator gateway`):

```text evidence="expected"
src/configs/  src/contracts/  src/filters/  src/guards/  src/middlewares/  src/routes/  src/services/
src/guards/jwt.guard.ts
src/main.ts
```

## Spring Boot service

Classes are PascalCase and the owning directory is the **last package segment**, so `com/acme/users/routes/CustomerController.java` is a controller.

| Kind | Directory | File pattern |
|---|---|---|
| controller | routes/ | `*Controller.java` |
| service | application/ | `*Service.java` |
| port | ports/ | `*Port.java` |
| adapter | adapters/ | `*Adapter.java` |
| mapper | adapters/ | `*Mapper.java` |
| repository | persistence/ | `*Repository.java` |
| event | events/ | `*Event.java` |
| listener | events/ | `*Listener.java` |
| dto | contracts/ | `*Dto.java` |
| config | configs/ | `*Config.java` |
| guard | security/ | `*Guard.java` |

Plain helper files without a recognised suffix are allowed anywhere and are ignored by discovery.

## What a violation looks like

Create a module folder with a controller but no `MODULE.pso.ts` and run `pso validate` (Expected output):

```text evidence="expected"
ERROR   PSO_MODULE_001 gateway: Module declaration missing.

Module: ghost

The directory routes/ghost/ contains HTTP code but no MODULE.pso.ts. There is no fallback discovery.

No route of this directory is activated.
FAILED
```

The message names the code, the project, the module and the consequence, and `validate` exits with code 1. The orchestrator would also refuse to start, so the problem cannot reach a running system.

## Working with the conventions

| You want | Do |
|---|---|
| a new HTTP module | `pso make:module <name>`; it creates the two required files |
| a controller, service, DTO, guard | `pso make:controller`, `make:service`, `make:dto`, `make:guard`; they place the file correctly and register it |
| a helper that is not a framework resource | name it without a known suffix (`format-date.ts`); discovery ignores it |
| to know what the tools think exists | `pso inspect --service <name>` lists the discovered resources with their files |

## Common mistakes

- Putting `users.controller.ts` directly in `src/` instead of `src/routes/users/`.
- Naming a module in camelCase or with underscores (names are kebab-case).
- Moving files by hand and forgetting to update `MODULE.pso.ts`; use the generators, or run `pso validate` immediately after.

## Explicit configuration

When convention is not enough (ports, limits, upstream addresses, secrets) there is explicit configuration, with documented precedence and a typed schema: see [Configuration resolution](concepts/configuration.md).

## Next steps

- [Structural discovery](concepts/structural-discovery.md)
- [Module composition](concepts/module-composition.md)
