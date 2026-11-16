# pso build

> Type-check Node services and package artifacts (npm pack / Spring Boot jar) with checksums into .pso/artifacts

Type-check Node services and package artifacts (npm pack / Spring Boot jar) with checksums into .pso/artifacts.

## Usage

```text
pso build [--service name] [--json]
```

## Flags

| Flag | What it does |
|---|---|
| `--service` | Build one registered service. |
| `--json` | Print machine-readable results. |

## Examples

```bash
pso build
pso build --service gateway
```

## What it changes

- Validates and compiles selected projects, then writes versioned artifacts and a checksum manifest under `.pso/artifacts/`.

## Illustrative output

This sample shows the shape of the result. Names, ports, digests and timings may differ in your workspace.

```text evidence="illustrative"
gateway   gateway-0.1.0.tgz  18234 bytes  sha256:ab12cd34ef56
```

## Verify the result

- Read `.pso/artifacts/manifest.json` to confirm the artifact paths and SHA-256 values.
- Use `pso deploy:plan` to inspect activation steps before deployment.

## Failure cases

- TypeScript errors fail the build (the orchestrator is compiled with tsc).
- Validation errors stop the build before anything is packaged.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [devops/build-and-deploy](devops/build-and-deploy.md)
