# pso make:route

> Add a decorated controller action AND its ROUTES.pso.ts declaration, kept consistent (rolled back if validation fails). With --upstream the action forwards the request to one service; with --compose it gets the UpstreamClient and the route declares the services it may call to build its response. With --upstream and --dto the body is validated at the gateway and then forwarded unchanged; a DTO with file fields generates an upload (forwardUpload); --download a streamed file download (forwardDownload)

Add a decorated controller action AND its ROUTES.pso.ts declaration, kept consistent (rolled back if validation fails). With --upstream the action forwards the request to one service; with --compose it gets the UpstreamClient and the route declares the services it may call to build its response. With --upstream and --dto the body is validated at the gateway and then forwarded unchanged; a DTO with file fields generates an upload (forwardUpload); --download a streamed file download (forwardDownload).

## Usage

```text
pso make:route <module> <action> --method GET --path / [--service <orchestrator>] [--controller name] [--public | --guard jwt --scopes a,b] [--upstream <service> | --compose <service,service>] [--dto <name>] [--download [--max-download 100MB]] [--dry-run] [--json]
```

## Flags

| Flag | What it does |
|---|---|
| `--method` | Select the HTTP method for the action. |
| `--path` | Set the path relative to the controller prefix. |
| `--service` | Choose the orchestrator in a multi-project workspace. |
| `--controller` | See the usage line and the task guide for this option. |
| `--public` | Mark an intentionally anonymous route where permitted. |
| `--guard` | Require the named guard. |
| `--scopes` | Require token scopes when using a suitable guard. |
| `--upstream` | Forward to a registered service instead of handling locally. |
| `--compose` | Give the action the UpstreamClient and declare the comma-separated services it may call to build its response (metadata.services). |
| `--dto` | Validate the JSON body against a DTO created with `pso make:dto`; with --upstream the validated request is then forwarded unchanged. |
| `--download` | See the usage line and the task guide for this option. |
| `--max-download` | See the usage line and the task guide for this option. |
| `--dry-run` | Preview the changes. |
| `--json` | See the usage line and the task guide for this option. |

## Examples

```bash
pso make:route users findAll --method GET --path / --public
pso make:route users findById --method GET --path /:id --guard jwt --scopes users:read
pso make:route users list --method GET --path /list --public --upstream catalog
```

## What it changes

- Updates `ROUTES.pso.ts` and the matching controller action together, then validates the resulting registry.
- The full public path combines the controller prefix and the action path. A decorator without a registry entry remains inaccessible.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
updated  src/modules/users/controllers/users.controller.ts
updated  src/modules/users/routes/ROUTES.pso.ts
```

## Verify the result

- Run `pso routes:list` for the public path and `pso routes:inspect users.findById` for policy and target.
- Send requests with and without the declared credentials to check runtime behavior.

## Failure cases

- `already has a member named`: existing code is never overwritten.
- `... is already declared`: duplicate routes are refused.
- `Refusing to declare a public non-GET route`.
- If the change would make the registry invalid (for example an unknown guard) it is rolled back and the diagnostics are printed.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [orchestrator/routes](orchestrator/routes.md)
- [orchestrator/controllers](orchestrator/controllers.md)
