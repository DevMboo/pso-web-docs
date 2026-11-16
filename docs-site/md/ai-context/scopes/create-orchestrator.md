# Task: create an orchestrator

> Exact steps to add the Node.js orchestrator to a workspace.

1. Work in the workspace root (the folder with `pso.workspace.json`). If there is none: `pso create workspace <name>` then `cd <name>`.
2. `pso create orchestrator <name>` (kebab-case). A workspace has exactly one orchestrator; if one exists, stop and tell the user.
3. In the new folder: `npm install`.
4. `pso validate` must print `OK`.
5. Configure the guard before expecting any guarded route to work: create `<name>/.env` (git-ignored) with `PSO_JWT_ISSUER`, `PSO_JWT_AUDIENCE` and `PSO_JWT_JWKS_URI` or `PSO_JWT_PUBLIC_KEY_FILE`. Never commit it and never invent values; ask the user for the issuer and audience.
6. Start with `pso dev`. With no modules only `/health/*` is served; `404` elsewhere is correct.

Do not edit `src/main.ts` to add routes. Add modules with `pso make:module`.
