# First orchestrator

> Generate the Node.js + TypeScript gateway, build and start it, and understand what its minimal entry point does at start-up and what it answers with no routes.

## 1. Command

```pso-example
pso create workspace demo
cd demo
pso create orchestrator gateway
pso validate
```

## 2. What was created

Real output (verified):

```text evidence="verified"
$ pso create orchestrator gateway
created    .gitignore
created    package.json
created    src/configs/.gitkeep
created    src/contracts/.gitkeep
created    src/filters/.gitkeep
created    src/guards/.gitkeep
created    src/guards/jwt.guard.ts
created    src/main.ts
created    src/middlewares/.gitkeep
created    src/routes/.gitkeep
created    src/services/.gitkeep
created    tsconfig.json
registered "gateway" in pso.workspace.json (port 3000)
```

| Path | Purpose |
|---|---|
| `package.json` | scripts `build`, `dev` (build, then start with `.env` loaded if present), `start`, `typecheck`; dependencies `@pso/core`, `@pso/http`, `@pso/security` (as `file:` paths to your checkout) and `reflect-metadata`; `engines.node >=24` |
| `tsconfig.json` | decorators and metadata on, output to `dist` |
| `.gitignore` | `node_modules`, `dist`, `.env` |
| `src/main.ts` | the whole entry point |
| `src/guards/jwt.guard.ts` | `JwtGuard.create()`; the logic lives in `@pso/security` |
| `src/routes/` | HTTP modules go here (empty for now) |
| `src/middlewares/ filters/ services/ configs/ contracts/` | other kinds of files, one directory each |

## 3. The entry point

```typescript title="gateway/src/main.ts"
import "reflect-metadata";
import { resolve } from "node:path";
import { bootstrap, log, shutdownOnSignals, StartupError } from "@pso/http";

bootstrap({ appDir: __dirname, workspaceFile: process.env["PSO_WORKSPACE"] ?? resolve(__dirname, "..", "..", "pso.workspace.json"), port: Number(process.env["PORT"] ?? 3000) })
  // SIGINT / SIGTERM: stop accepting, let running requests finish within http.shutdown.graceMs, release the dependents, exit 0.
  // The process is ended with exit code 1 only after app.shutdown.deadlineMs (grace period + cleanup time), never earlier.
  .then((app) => { shutdownOnSignals(app); })
  .catch((e: Error) => {
    log("error", "startup failed", { error: e.message, diagnostics: e instanceof StartupError ? e.diagnostics.map((d) => d.code) : undefined });
    process.exit(1);
  });
```

Line by line: `bootstrap()` does everything; the `.then` installs the shutdown handlers (on `SIGINT` or `SIGTERM`: stop accepting connections, let running requests finish within the grace period, release background work, exit 0; if that has not finished after the grace period plus the cleanup time, the process is ended with exit code 1: see [Runtime lifecycle](concepts/runtime-lifecycle.md)); the `.catch` turns a start-up failure into one log line and a non-zero exit, so a process manager sees it.

## 4. What `bootstrap()` does

1. Reads the logging, limits and security configuration; an invalid value stops the start.
2. Discovers the compiled modules under `dist/routes` and validates them with the same rules as `pso validate`.
3. Resolves providers and controllers and activates **only** the routes that `ROUTES.pso.ts` declares.
4. Opens the listener. With no modules it serves only the framework health endpoints.

Details: [Bootstrap](orchestrator/bootstrap.md).

## 5. Build and start

```powershell
cd gateway
npm install
npm run dev
```

The orchestrator is **compiled** (`tsc`) because decorators cannot run directly in Node. `npm run dev` builds and starts; `pso dev` does the same for the whole workspace.

On a terminal you see (the first line and the format are verified output from a real run; the counts shown are for an orchestrator with no modules and are expected values; the logger is described in [Logging](devops/logging.md)):

```text evidence="verified"
2026-11-08 14:54:30.517 INFO  [startup] starting orchestrator  service=gateway environment=development logLevel=info logFormat=pretty node=v24.18.0
2026-11-08 14:54:30.531 INFO  [security] security initialized (pem, ES256)
2026-11-08 14:54:30.537 INFO  [http] orchestrator listening  host=127.0.0.1 port=3000 routes=2 modules=0
```

When the output is not a terminal (a pipe, a file, CI) the same events are one JSON object per line.

## 6. Verify

```bash
curl -i http://127.0.0.1:3000/health/ready
```

```text evidence="expected"
HTTP 200 application/json; charset=utf-8
{"status":"UP","upstreams":1}
```
*(expected shape; `upstreams` is the number of upstream services declared in the workspace: it was 1 in the captured run, and is 0 for a workspace with only the orchestrator)*

Everything else is `404` with `application/problem+json` and `"code":"PSO_HTTP_NOT_FOUND"`: no route has been declared yet.

## 7. Variations

| You want | Do |
|---|---|
| another port | `PORT=4000 npm run dev` (and update `pso.workspace.json` so other tools agree) |
| more detail while developing | `PSO_LOG_LEVEL=debug npm run dev` |
| plain text without colour | `NO_COLOR=1 npm run dev` |
| to start it without `npm` | `node dist/main.js` after `npm run build` |

## 8. Common mistakes

- Running `node src/main.ts`: it is not valid; use `npm run dev` or `pso dev`.
- Creating a second orchestrator: a workspace has exactly one public entry point.
- Forgetting `npm install`: `pso dev` reports `dependencies are not installed`; pass `--install` to let it run `npm install`.
- `startup failed ... diagnostics=["PSO_..."]`: the registry or security configuration is invalid. Run `pso validate` for the full message.

## Next steps

- [First microservice](getting-started/first-microservice.md)
- [First endpoint](getting-started/first-endpoint.md)
- [Orchestrator architecture](orchestrator/architecture.md)
