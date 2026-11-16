# pso make:provider

> Generate an injectable provider class in src/providers/ and, with --module, list it in that module's providers (explicit registration: a provider nobody lists is never constructed)

Generate an injectable provider class in src/providers/ and, with --module, list it in that module's providers (explicit registration: a provider nobody lists is never constructed).

## Usage

```text
pso make:provider <name> [--module <module>] [--service <orchestrator>] [--dry-run] [--json]
```

## Flags

`--module`, `--service`, `--dry-run`, `--json`.

## Examples

```bash
pso make:provider clock --module users --service gateway --dry-run
pso make:provider clock --module users --service gateway
```

## What it changes

- Writes `src/providers/clock.provider.ts`: an `@Injectable()` class the framework constructs once per application and injects by constructor type.
- `--module users` adds the class to `providers` of that module's `MODULE.pso.ts`. Registration is explicit: a provider that no module lists is never constructed.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
created  src/providers/clock.provider.ts
updated  src/routes/users/MODULE.pso.ts
```

## Verify the result

- Inject it in a controller or service of the module by constructor type, then run `pso validate`.

## Failure cases

- `Module "users" does not exist`: create it with `pso make:module users`.
- Without `--module` the file is written and the command says that nothing uses it yet.
- `PSO_MODULE_006` from `pso validate`: a class injects the provider but its module does not list it.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [orchestrator/services-and-providers](orchestrator/services-and-providers.md)
- [orchestrator/dependency-injection](orchestrator/dependency-injection.md)
