# Protect an API with JWT scopes

> Configure a trusted issuer, declare public and protected routes, and diagnose 401 versus 403.

The gateway verifies callers before forwarding a protected route. The Spring service verifies the bearer token again: the service does not trust the gateway's network position as proof of identity.

## 1. Configure a verifier

Use an identity provider's issuer, audience and HTTPS JWKS URI. This example uses placeholders; replace them with values from your provider:

```json title="gateway/pso.config.json" evidence="illustrative"
{
  "security": {
    "authentication": {
      "provider": "jwt",
      "issuer": "https://idp.example.com",
      "audience": "shop-api",
      "algorithms": ["ES256"],
      "jwks": { "uri": "https://idp.example.com/.well-known/jwks.json" }
    }
  }
}
```

The generated Spring service reads its `PSO_JWT_*` environment settings; configure them for the same issuer, audience and key source. For local keys, use `pso security:keys --generate` and [the PEM setup](security/keys-and-pem.md). Never commit a private key or bearer token.

## 2. Declare access per route

In `ROUTES.pso.ts`, a health or other intentionally anonymous endpoint uses `security: { public: true }`. A protected operation uses `security: { guard: "jwt", scopes: ["products:read"] }`. The exact route shape is shown in [Secure routing](security/secure-routing.md). A decorator cannot weaken the registry policy. Public and guarded on the same route is an error.

## 3. Check behavior

Run `pso security:check` and `pso validate`, then start the workspace. Call the public route without a token; it should work. Call the protected route without a token; expect `401`. A valid token lacking the required scope is authenticated but not authorized, so expect `403`. A valid token with the scope reaches the handler. Use a test identity provider or `pso-test` in Spring tests; do not paste real tokens into documentation or logs.

If every protected request is `401`, compare issuer, audience, allowed algorithms and the signing key in **both** processes. If only a scoped request is `403`, inspect the token's scope claim and route declaration. For a corporate verifier with different credentials, see [Custom guards](security/authentication.md#custom-guards); it requires explicit configuration.

## What you will see

Expected outputs from a gateway protected this way, with a token minted for the test (issuer, audience and signing key were test values):

```text evidence="expected"
GET /reports/summary   (public route)                       -> 200
GET /customers         (no token)                           -> 401 PSO_AUTH_UNAUTHORIZED
GET /customers         (valid token, wrong scope)           -> 403 PSO_AUTH_FORBIDDEN
GET /customers         (valid token, scope customers:read)  -> forwarded to the service
```

and the orchestrator says *why* in its log (never in the response body):

```text evidence="expected"
WARN  [security] authentication failed  reason=missing_header route="GET /customers"
WARN  [security] authorization denied (doc-corr-0003)  missing=scope:customers:read route="GET /customers"
```

`pso security:check` reads the same settings as the orchestrator: the `security.authentication.*` keys shown above, the legacy `security.jwt.*` keys (mapped onto them) and the `PSO_JWT_*` variables. With the configuration of step 1 in place it reports no `SEC005`. If you have not configured the verifier yet, it says so before you run anything:

```text evidence="expected"
HIGH     SEC005 gateway
         jwt guard is used but security.authentication.issuer, security.authentication.audience, a key source (security.authentication.jwks.uri or security.authentication.publicKey.file) is not configured for "production"; the orchestrator will refuse to start
```

## Variations

| You want | Do |
|---|---|
| a local development key | `pso security:keys --generate`, point `PSO_JWT_PUBLIC_KEY_FILE` (orchestrator) or a loopback `PSO_JWT_JWKS_URI` (Spring) at the public key ([keys](security/keys-and-pem.md)) |
| a role instead of a scope | `security: { guard: "jwt", roles: ["admin"] }` |
| the same check in the Spring service | `@PreAuthorize("hasAuthority('SCOPE_products:read')")`, already generated ([security integration](microservices/security-integration.md)) |
| a custom verifier | a custom guard with an explicit `custom` provider declaration ([authentication](security/authentication.md)) |

## 401 or 403: a quick guide

| Check | 401 | 403 |
|---|---|---|
| is a token present and well-formed? | no | yes |
| is it signed by a key you trust, with the right issuer and audience, unexpired? | no | yes |
| does it carry the scope or role the route needs? | not evaluated | **no** |

## Getting a token without an identity provider

Every request in this tutorial needs a signed JWT. On a development machine the CLI provides one:

```bash
pso idp:dev init
pso idp:dev serve
```

`serve` prints the `PSO_JWT_*` lines to put in the orchestrator's and the services' `.env`. In another terminal:

```bash
pso dev
curl -i http://127.0.0.1:3000/customers
curl -i -H "Authorization: Bearer $(pso idp:dev token --scopes customers:read)" http://127.0.0.1:3000/customers
curl -i -H "Authorization: Bearer $(pso idp:dev token --scopes other:scope)" http://127.0.0.1:3000/customers
```

The three answers are `401` (no token), `200` and `403` (a valid token without the scope). The issuer is for development only; see [authentication](security/authentication.md) for what keeps it out of production.

## Common mistakes

- Making a route public to get past a `401`.
- Using one broad scope for every route.
- Configuring the gateway and forgetting the Spring service: both verify.

## Troubleshooting

| Symptom | Cause | Check | Fix |
|---|---|---|---|
| the orchestrator will not start: `PSO_SEC_MISSING`, `PSO_SEC_ALGORITHM`, `PSO_SEC_PEM_PATH` or `PSO_SEC_JWKS_INSECURE` | incomplete or unsafe JWT settings | the log's `code` | [Why the orchestrator refuses to start](reference/startup-failures.md) has each one reproduced |
| `401` with a token you just made | issuer, audience, algorithm or expiry differ, or the key does not match | `pso security:validate` | align the values |
| `403` | the token lacks the scope | the gateway's "authorization denied" line | issue the scope |
| a Spring service ignores your key file | Spring reads only `PSO_JWT_JWKS_URI` | the service's start-up error | serve `keys/jwks.json` on loopback ([CRUD tutorial](tutorials/crud-api.md#12-give-both-services-a-way-to-verify-tokens)) |
