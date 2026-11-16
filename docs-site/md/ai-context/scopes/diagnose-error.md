# Task: diagnose a PSO error

> How to find and fix the cause of a PSO diagnostic or a failing request.

1. Reproduce with `pso validate` (add `--json` for machine output). Read the **whole** diagnostic: for route errors it shows Module, Route, Controller, Action, Expected and Found.
2. Fix the cause in the source of truth. Mismatch between decorator and declaration: align them (prefer `pso make:route` for new routes). Never delete a guard, add `public: true` or loosen a policy to make an error disappear.
3. Symptoms:
   - decorated method answers 404: it is not declared in `ROUTES.pso.ts`.
   - every guarded request answers 401: the guard has no `PSO_JWT_*` configuration or the token's issuer/audience differ; check the start-up log.
   - orchestrator will not start: route registry invalid; read the logged `PSO_*` codes.
   - `pso dev` port in use / dependencies missing: free the port / `--install`.
   - 504 from a route: the upstream service is not running.
4. Re-run `pso validate`, `pso routes:list`, `pso security:check`.

Diagnostic codes of this version:

- `PSO_BAD_NAME` — "…" is not …-case
- `PSO_CONFIG_PLAINTEXT_SECRET` — "…" looks like a secret stored as plain text
- `PSO_CONFIG_UNKNOWN_KEY` — Unknown configuration key "…"
- `PSO_CONFIG_UNREADABLE` — Cannot read configuration: …
- `PSO_CONTRACT_DRIFT` — Contract "…" differs from its copy in service "…"
- `PSO_CONTRACT_INVALID_JSON` — Cannot parse contract: …
- `PSO_CONTRACT_OPERATION` — Each operation needs string id, method and a path starting with "/"
- `PSO_CONTRACT_SHAPE` — Contract must be a JSON object
- `PSO_CONTRACT_UNKNOWN_SERVICE` — Contract targets unknown service "…"
- `PSO_CONTRACT_VERSION` — Contract version "…" is not semver
- `PSO_CYCLE` — Circular service dependency: …
- `PSO_DISPATCH_012` — …: dispatch is enabled but no provider is selected
- `PSO_DISPATCH_013` — …: provider "…" is configured but ${d.installed ? 
- `PSO_DISPATCH_014` — …: the … adapter is installed but dispatch is disabled (nothing connects)
- `PSO_DISPATCH_015` — …: dispatch is enabled but no events are declared
- `PSO_DUPLICATE_ID` — Duplicate … "…" (also declared in …)
- `PSO_DUPLICATE_SERVICE` — Duplicate service "…"
- `PSO_FEI_ORCHESTRATOR_MISSING` — the FEI talks to orchestrator "…" but this workspace has none
- `PSO_FEI_ORCHESTRATOR_UNKNOWN` — … names orchestrator "…" but the workspace's orchestrator is "…"
- `PSO_FRONTEND_CONTRACT_STALE` — …: … is from orchestrator contract …, the orchestrator is now at …
- `PSO_FRONTEND_PATH` — Frontend "…": path must be relative and stay inside the workspace
- `PSO_FRONTEND_PORT` — Frontend "…": port must be an integer between 1024 and 65535
- `PSO_FRONTEND_SHAPE` — "frontends" must be an array
- `PSO_JWT_ISSUER` — PSO_JWT_AUDIENCE
- `PSO_MEI_DIRECT_URL` — …: … calls an absolute URL with fetch: backend calls go through usePsoContext().api (orchestrator only)
- `PSO_MEI_OWN_ROUTER` — …: … uses …: a microfrontend renders inside the router of its host and must not create its own history
- `PSO_MISPLACED_RESOURCE` — … "…" must live in ${spec.exact ? 
- `PSO_MISSING_DEPENDENCY` — Service "…" depends on unknown service "…"
- `PSO_MODULE_001` — Module declaration missing.
- `PSO_MODULE_002` — Route registry missing.
- `PSO_MODULE_003` — Controller is not declared in MODULE.pso.ts.
- `PSO_MODULE_004` — Duplicate module name.
- `PSO_MODULE_005` — Module does not reference its route registry.
- `PSO_MODULE_006` — Dependency cannot be resolved.
- `PSO_MODULE_007` — Circular provider dependency.
- `PSO_MODULE_008` — Imported module not found.
- `PSO_NO_SOURCE_ROOT` — Source root "…" does not exist
- `PSO_PORT_CONFLICT` — Services "…" and "…" both use port …
- `PSO_ROUTE_001` — Route references a controller that is not part of the module.
- `PSO_ROUTE_002` — Controller action does not exist.
- `PSO_ROUTE_003` — Controller is not decorated.
- `PSO_ROUTE_004` — Route declaration mismatch.
- `PSO_ROUTE_005` — Route path does not match the controller.
- `PSO_ROUTE_006` — Duplicate or conflicting route declaration.
- `PSO_ROUTE_007` — Security policy conflict.
- `PSO_ROUTE_008` — Unknown ….
- `PSO_ROUTE_009` — Invalid route declaration.
- `PSO_ROUTE_010` — Route targets an unknown service.
- `PSO_SCHEMA_VERSION` — Unsupported schemaVersion … (this CLI supports 1)
- `PSO_SERVICE_DEV` — Service "…": dev.command must be a non-empty array of strings
- `PSO_SERVICE_MISSING` — Service "…" path "…" does not exist
- `PSO_SERVICE_PATH` — Service "…": path must be relative and stay inside the workspace
- `PSO_SERVICE_PORT` — Service "…": port must be an integer between 1024 and 65535
- `PSO_SERVICE_PREFIX` — Service "…": …
- `PSO_SERVICE_SHAPE` — Service name must be kebab-case: …
- `PSO_WORKSPACE` — PSO_SERVICE
- `PSO_WORKSPACE_SHAPE` — Workspace file must be a JSON object
- `PSO_WORKSPACE_UNREADABLE` — Cannot read …: …
