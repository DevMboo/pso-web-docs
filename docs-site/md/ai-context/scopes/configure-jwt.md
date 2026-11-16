# Task: configure JWT

> Exact steps to configure token validation without exposing secrets.

1. Ask the user which identity provider issues tokens: you need its **issuer**, the **audience** for this API and its **JWKS URI** (https). Do not invent them.
2. Orchestrator: set `PSO_JWT_ISSUER`, `PSO_JWT_AUDIENCE`, `PSO_JWT_JWKS_URI`, optionally `PSO_JWT_ALGORITHMS` (for example `ES256`) in `<orchestrator>/.env` (git-ignored) or the process environment.
3. Spring services: provide the same four variables to every service.
4. Local development without a provider: `pso security:keys --generate --alg ES256`, then use `PSO_JWT_PUBLIC_KEY_FILE=../keys/<kid>.public.pem` with `PSO_JWT_ALGORITHMS=ES256`. These keys are for development only. Never read, print or commit `*.private.pem`.
4b. The same values may be declared in `pso.config.json` under `security.authentication.*` (the variables override it). Do not add JWT verification code to the project: it belongs to `@pso/security`.
5. `pso config:validate` and `pso security:check` must pass (SEC005 means security is not configured and the orchestrator will refuse to start). A bad configuration stops start-up with `PSO_SEC_*` codes.
6. Verify behavior: no token `401`; expired, wrong issuer, wrong audience or wrong signature `401`; missing scope `403`.
7. Never switch to a symmetric algorithm (`HS256`), never accept `alg: none`, never put a plain-http JWKS in production.
