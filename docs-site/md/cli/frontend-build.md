# pso frontend:build

> Build the FEI and/or MEIs independently (hashed assets); a MEI also gets its release manifest (file hashes, contract, shared versions, optional ed25519 signature)

Build the FEI and/or MEIs independently (hashed assets); a MEI also gets its release manifest (file hashes, contract, shared versions, optional ed25519 signature).

## Usage

```text
pso frontend:build [<project>] [--base-path /commerce/ | --env <name>] [--version 1.0.0] [--allow-dev-token] [--sign-key <pem-file> --key-id <id>] [--json]
```

## Flags

`--base-path`, `--env`, `--version`, `--allow-dev-token`, `--sign-key`, `--key-id`, `--json`.

## Examples

```bash
pso frontend:build
pso frontend:build customers --base-path /commerce/
pso frontend:build --base-path /commerce/ --sign-key release.pem --key-id release-1
```

## What it changes

- Production build with hashed file names and integrity hashes. A MEI also gets `dist/pso-manifest.json`: every file with its SHA-256, the shared-context contract, the shared dependency versions and, with `--sign-key`, an Ed25519 signature.
- `--allow-dev-token` builds a validation shell that accepts a pasted token and shows a warning banner.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
fei-commerce          2.4 s     171.3 KB gzip
mei-customers         2.6 s     209.5 KB gzip   release 0.1.0 (signed)
```

## Failure cases

- A `shellKb` budget in `FEI.pso.ts` that is exceeded fails the build.
- `--sign-key` and `--key-id` go together.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [frontend/releases](frontend/releases.md)
- [frontend/styling-and-performance](frontend/styling-and-performance.md)
