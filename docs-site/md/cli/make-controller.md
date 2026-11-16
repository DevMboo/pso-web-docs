# pso make:controller

> Generate a controller (orchestrator: inside a module; Spring: hexagonal layout)

Generate a controller (orchestrator: inside a module; Spring: hexagonal layout).

## Usage

```text
pso make:controller <module> [name] [--prefix /path] [--service <orchestrator>] [--dry-run] [--json] (Spring: pso make:controller <name> --service <svc> [--route /path] [--method GET] [--dry-run] [--force] [--json])
```

## Flags

| Flag | What it does |
|---|---|
| `--prefix` | Choose the controller's URL prefix. |
| `--service` | Choose the orchestrator when the workspace has more than one project. |
| `--dry-run` | Preview the generated files. |
| `--json` | See the usage line and the task guide for this option. |
| `--route` | See the usage line and the task guide for this option. |
| `--method` | See the usage line and the task guide for this option. |
| `--force` | Allow replacement of a conflicting generated file. |

## Examples

```bash
pso make:controller users
pso make:controller users --prefix /people
```

## What it changes

- Creates a controller in an existing module and updates the module's controller registration.
- A controller prefix and decorators describe methods; only `ROUTES.pso.ts` exposes them.

## Verify the result

- Run `pso validate`, then inspect the controller list in `MODULE.pso.ts`.
- Add a route and check `pso routes:list` before making an HTTP request.

## Failure cases

- `Module "x" does not exist`: run `pso make:module` first.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [orchestrator/controllers](orchestrator/controllers.md)
