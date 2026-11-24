# pso workspace:migrate

> Migrate a workspace to 0.2.0: add pso.infra.json, adopt the existing development TLS authority, certificates and identity key (reused, not regenerated), provision the local environment, and add the PUBLIC rules that Spring services generated before 0.2.0 lack for routes the orchestrator declares public (with backups). Without --apply it only reports; application logic, declarations and data are never changed

Migrate a workspace to 0.2.0: add pso.infra.json, adopt the existing development TLS authority, certificates and identity key (reused, not regenerated), provision the local environment, and add the PUBLIC rules that Spring services generated before 0.2.0 lack for routes the orchestrator declares public (with backups). Without --apply it only reports; application logic, declarations and data are never changed.

## Usage

```text
pso workspace:migrate [--apply] [--identity keycloak|pso-dev|none] [--json]
```

## Flags

`--apply`, `--identity`, `--json`.

## Examples

```bash
pso workspace:migrate
pso workspace:migrate --apply
pso workspace:migrate --apply --identity keycloak --json
```

## What it changes

- Inspection without `--apply`. With it: writes pso.infra.json, adopts a valid `tls/` authority and its certificates and the `pso idp:dev` key (reused, not regenerated), excludes .infra/ from git and provisions the local environment. Application code, declarations and data are never changed.
- Spring security: compares each Spring service's `pso.security.rules` with the routes the workspace declares and adds only the missing rules for declared public routes, at the end of the list, marked `# added by pso workspace:migrate (0.2.0)`. Custom rules are kept; a backup goes to `.infra/backups/`; a second run adds nothing.
- A workspace that already has pso.infra.json skips the infrastructure steps and still checks the Spring rules; the result is recorded in `.infra/migration-0.2.0.json`.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
migration to 0.2.0 of "pso-reference" (inspection: nothing is written)
  note: pso.infra.json exists: the infrastructure steps are done (pso infra:plan); only the remaining steps run
Spring security (routes the orchestrator declares public):
  ok       catalog: GET /catalog/products -> rule at line 71 (/catalog/products) already makes it public

Nothing to migrate.
```

## Failure cases

- `conflict`: an explicit rule gives the public path another access; the developer's rule is kept. Decide which one is right.
- `ambiguous`: no application.yml, or a `pso.security.rules` block that cannot be read with certainty; the message gives the rule to add by hand.
- `exposed-extra`: a service rule makes PUBLIC a path the orchestrator protects; reported, never removed automatically.
- Frontend declarations with errors block the migration (pso frontend:validate).

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [infrastructure/migration](infrastructure/migration.md)
