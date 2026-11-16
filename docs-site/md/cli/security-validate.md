# pso security:validate

> Validate the security configuration of the orchestrator (security.authentication.*) and of every PSO Spring service (pso.security.*) before anything runs

Validate the security configuration of the orchestrator (security.authentication.*) and of every PSO Spring service (pso.security.*) before anything runs.

## Usage

```text
pso security:validate [--env development|test|staging|production] [--json]
```

## Flags

`--env`, `--json`.

## Examples

```bash
pso security:validate --env production
pso security:validate --json
```

## Failure cases

- Invalid orchestrator or Spring security settings are reported before startup; correct the named setting rather than relaxing a route.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [security/security-configuration](security/security-configuration.md)
- [spring/security-and-cors](spring/security-and-cors.md)
