# Service contracts

> Versioned JSON contracts mirrored into each service and checked for drift - what is generated, what is validated, how to change one safely, and what contracts deliberately do not cover.

`pso make:resource customer --service catalog --route /customers` creates the contract in two places:

```text evidence="verified"
created    gateway/src/contracts/customer.contract.json
created    catalog/src/main/java/com/pso/catalog/contracts/CustomerDto.java
created    catalog/src/main/java/com/pso/catalog/routes/CustomerController.java
created    catalog/src/main/resources/contracts/customer.contract.json
created    catalog/src/test/java/com/pso/catalog/routes/CustomerControllerTests.java
created    docs/api/customer.md
created    gateway/src/routes/customer/MODULE.pso.ts
created    gateway/src/routes/customer/ROUTES.pso.ts
created    gateway/src/routes/customer/customer.controller.ts
```

The two contract files (`gateway/src/contracts/` and `catalog/src/main/resources/contracts/`) are identical copies; the rest of the output is the controller, DTO, test, API note and gateway module that go with them.

```json title="customer.contract.json (generated)"
{
  "name": "customer",
  "version": "0.1.0",
  "service": "catalog",
  "operations": [{ "id": "customerList", "method": "GET", "path": "/customers" }]
}
```

## Validation

`pso validate` checks that the contract is valid JSON of the expected shape, has a semantic version, unique operation ids and a `service` that is declared in the workspace, and that the two copies are byte-identical (`PSO_CONTRACT_DRIFT` otherwise). Reproduced here by changing `version` in the gateway copy only: changing `version` in the gateway copy only makes `pso validate` print

```text evidence="verified"
ERROR   PSO_CONTRACT_DRIFT gateway/src/contracts/customer.contract.json: Contract "customer" differs from its copy in service "catalog"
        hint: Update both copies together (pso make:resource --force regenerates them)
gateway: 5 resource(s), digest 618af77aaa00
catalog: 5 resource(s), digest 484037ac8abd
FAILED
```

and exit with code 1. Regenerating with `--force` rewrites the copy you changed (`overwritten gateway/src/contracts/customer.contract.json`; the other files are reported `unchanged`) and `pso validate` is `OK` again.

## Changing a contract: step by step

1. Decide whether the change is observable by callers (add an operation, rename a path) or not (documentation).
2. Edit **both** copies, or regenerate: `pso make:resource customer --service catalog --route /customers --force` (review the diff before committing; `--force` overwrites what the generator owns).
3. Bump `version`: minor for additions, major for removals or renames, patch for metadata.
4. Run `pso validate`: it must print `OK`.
5. Change the Spring controller and the gateway route in the same commit, so the three agree.

## Variations

| Need | Contract change |
|---|---|
| a new read endpoint | add an operation with a new `id`, bump minor |
| two services expose the same resource name | give each its own contract file; `name` and `service` identify the pair |
| a deprecated operation | keep it for a release, mark the replacement; remove with a major bump |

## What contracts do not cover

Contracts describe **operations**, not payload schemas. Payloads are defined by the Spring DTOs (with Jakarta Validation) and, when the gateway answers a request itself, by `@Dto` classes ([DTO validation](orchestrator/dto-validation.md)). The two are not cross-checked against the contract in this version, so a payload change needs its own review.

## Common mistakes

- Editing one copy.
- Reusing an operation id for a different method or path.
- Treating a green `validate` as proof that the payloads match.

## Next steps

- [Contracts (concept)](concepts/contracts.md)
- [First endpoint](getting-started/first-endpoint.md)
