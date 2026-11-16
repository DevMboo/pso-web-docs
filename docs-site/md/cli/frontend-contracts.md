# pso frontend:contracts

> Generate typed frontend contracts from the orchestrator's real route metadata and DTO classes into every frontend project (src/models/contract.generated.ts); --check fails when a project's copy is stale

Generate typed frontend contracts from the orchestrator's real route metadata and DTO classes into every frontend project (src/models/contract.generated.ts); --check fails when a project's copy is stale.

## Usage

```text
pso frontend:contracts [--check] [--fail-on-breaking] [--json]
```

## Flags

`--check`, `--fail-on-breaking`, `--json`.

## Examples

```bash
pso frontend:contracts
pso frontend:contracts --check
```

## What it changes

- Generates `src/models/contract.generated.ts` in every frontend project from the orchestrator's real routes and request DTOs, and `frontend/contracts/orchestrator.contract.json`. Response types are not generated because the orchestrator does not publish them.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
orchestrator contract 3c1ca82a668b: 14 operation(s), 6 request DTO(s)
updated fei-commerce, mei-customers, mei-catalog, mei-orders
```

## Failure cases

- `--check` exits 1 when a project's copy is stale.
- The orchestrator's routes must validate first.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [frontend/authentication-and-api](frontend/authentication-and-api.md)
