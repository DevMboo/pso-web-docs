# pso environment:privileges

> The EFFECTIVE sudo privilege of the agent's user: none, restricted (exact commands), broad (wildcards, shells, editors), unrestricted or unknown. A privileged command that works is never read as least privilege

The EFFECTIVE sudo privilege of the agent's user: none, restricted (exact commands), broad (wildcards, shells, editors), unrestricted or unknown. A privileged command that works is never read as least privilege.

## Usage

```text
pso environment:privileges <application> [--json]
```

## Flags

`--json`.

## Examples

```bash
pso environment:privileges shop-gateway
```

## What it changes

- Assesses the EFFECTIVE sudo privilege of the agent's user (none, restricted, broad, unrestricted, unknown).

## Failure cases

- Exit 1 unless the privilege is verifiably narrow or absent. `unknown` means the rules could not be listed without a password: verify them manually.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).
