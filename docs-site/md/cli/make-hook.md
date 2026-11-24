# pso make:hook

> Generate a lifecycle hook (onReady / onShutdown) in src/hooks/ and, with --module, list it in that module's providers (a hook nobody lists never runs)

Generate a lifecycle hook (onReady / onShutdown) in src/hooks/ and, with --module, list it in that module's providers (a hook nobody lists never runs).

## Usage

```text
pso make:hook <name> [--module <module>] [--service <orchestrator>] [--dry-run] [--json]
```

## Flags

`--module`, `--service`, `--dry-run`, `--json`.

## Examples

```bash
pso make:hook audit --module users --service gateway --dry-run
pso make:hook audit --module users --service gateway
```

## What it changes

- Writes `src/modules/users/hooks/audit.hook.ts`: an injectable class implementing `onReady` (after start-up succeeded, before the application reports ready; throwing stops start-up) and `onShutdown` (after running requests finished, before the framework releases its resources).
- `--module users` adds the class to `providers` of that module's `MODULE.pso.ts`. A hook runs only when a module lists it: a file in a `hooks/` folder is never run on its own.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
updated  src/modules/users/MODULE.pso.ts
created  src/modules/users/hooks/audit.hook.ts
```

## Verify the result

- Start the orchestrator: the log has `hook ready` and `lifecycle hooks ran` before `orchestrator listening`.
- `pso validate` checks the module declaration.

## Failure cases

- `Module "users" does not exist`: create it with `pso make:module users`.
- `In the modular structure a hook belongs to a module`: pass `--module <module>`.
- `A hook is generated in the orchestrator`: Spring services use Spring's own lifecycle callbacks.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [concepts/runtime-lifecycle](concepts/runtime-lifecycle.md)
- [orchestrator/services-and-providers](orchestrator/services-and-providers.md)
