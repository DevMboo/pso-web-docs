# Task: create a REST module

> Exact steps to add a module with controller, service and declared routes.

1. `pso make:module <name> --service <orchestrator>` creates `MODULE.pso.ts` and an empty `ROUTES.pso.ts`. Nothing is exposed yet.
2. `pso make:controller <name> --service <orchestrator>` creates `<name>.controller.ts` and lists it in `MODULE.pso.ts`.
3. Optional: `pso make:service <name> --service <orchestrator> --export` and `pso make:dto <name> <dto-name> --service <orchestrator>`.
4. For each endpoint run `pso make:route <name> <action> --method GET --path / --public --service <orchestrator>` (or `--guard jwt --scopes x:read` for protected routes). This writes the decorated action and the `ROUTES.pso.ts` entry together.
5. If the endpoint belongs to a Spring service, use `pso make:resource` instead (see the protected-route task).
6. `pso validate` then `pso routes:list`. Only routes listed there exist.

Do not hand-write routes in `ROUTES.pso.ts` unless the generator cannot express them. If you must, keep literals only and re-run `pso validate`.
