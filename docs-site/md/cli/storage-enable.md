# pso storage:enable

> Opt a Spring service into PSO Storage: adds the storage starter (and the S3 or Azure SDK), the pso.storage settings and multipart uploads; S3/Azure services use the local provider in the local and test profiles

Opt a Spring service into PSO Storage: adds the storage starter (and the S3 or Azure SDK), the pso.storage settings and multipart uploads; S3/Azure services use the local provider in the local and test profiles.

## Usage

```text
pso storage:enable <local|s3|azure> [--service <name>] [--root ./storage] [--bucket <b>] [--region <r>] [--container <c>] [--endpoint <url>] [--max-size 5MB] [--dry-run] [--json]
```

## Flags

| Flag | What it does |
|---|---|
| `--service` | Choose the Spring service. |
| `--root` | Relative local storage root (default ./storage). |
| `--bucket` | S3 bucket name. |
| `--region` | AWS region. |
| `--container` | Azure container name. |
| `--endpoint` | Azure blob endpoint or an S3-compatible emulator. |
| `--max-size` | pso.storage.upload.max-size (default 5MB). |
| `--dry-run` | Preview the changes. |
| `--json` | Print machine-readable results. |

## Examples

```bash
pso storage:enable local --service hr-service
pso storage:enable s3 --service hr-service --bucket acme-hr-documents --region eu-west-1 --dry-run
pso storage:enable azure --service hr-service
```

## What it changes

- Adds `pso-storage-spring-boot-starter` (and the S3 or Azure SDK, versioned by `pso-bom`), writes the `pso.storage` block and enables multipart uploads.
- S3 and Azure services get a local provider in the local and test profiles; no credential is ever written.

## Verify the result

- Run `pso storage:validate --service <name>`; start the service and read the `psoStorage` health component.

## Failure cases

- Only Spring services are supported. A root outside the service, an invalid bucket or container name and a non-HTTPS endpoint are refused before anything is written; running it again changes nothing.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [spring/storage](spring/storage.md)
