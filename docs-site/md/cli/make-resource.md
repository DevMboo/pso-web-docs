# pso make:resource

> Generate a connected route + contract in the orchestrator and controller + DTO + test in a Spring service

Generate a connected route + contract in the orchestrator and controller + DTO + test in a Spring service.

## Usage

```text
pso make:resource <name> --service <spring-service> [--route /customers] [--method GET|POST] [--guard jwt] [--scopes a,b] [--public] [--dry-run] [--force] [--json]
```

## Flags

| Flag | What it does |
|---|---|
| `--service` | Choose the owning Spring service already registered in the workspace. |
| `--route` | Set the public gateway path. |
| `--method` | Choose the generated GET collection or POST operation. |
| `--guard` | Name the route guard; `jwt` is the generated default. |
| `--scopes` | Require one or more token scopes. |
| `--public` | Declare an intentionally anonymous route where permitted. |
| `--dry-run` | Preview the files without writing them. |
| `--force` | Allow replacement of conflicting generated files. |
| `--json` | Print machine-readable results. |

## Examples

```bash
pso make:resource customer --service catalog --route /customers --scopes customers:read
pso make:resource order --service catalog --method POST --route /orders --scopes orders:write
```

## What it changes

- Creates a gateway module, route registry and forwarding controller together with a Spring controller, DTO, mirrored contract, test and API page.
- The generated handler is a stub. Replace its business behavior before claiming that the resource stores data.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
created    gw/src/routes/customer/MODULE.pso.ts
created    gw/src/routes/customer/ROUTES.pso.ts
created    gw/src/routes/customer/customer.controller.ts
created    catalog/src/main/java/.../CustomerController.java
...
```

## Verify the result

- Run `pso validate` for contract and registry consistency.
- Use `pso routes:inspect customer` for the target and access policy; then start both services and call the route.

## Failure cases

- All-or-nothing: if any target file exists with different content, nothing is written (use `--force`).
- Only GET (collection) and POST are generated; the orchestrator matches exact paths.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [getting-started/first-endpoint](getting-started/first-endpoint.md)
- [microservices/service-contracts](microservices/service-contracts.md)
