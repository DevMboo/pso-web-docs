# pso make:files

> Generate a Spring files controller (upload, download, metadata, list, move, delete on PsoStorageService with scope checks) and its test

Generate a Spring files controller (upload, download, metadata, list, move, delete on PsoStorageService with scope checks) and its test.

## Usage

```text
pso make:files <name> --service <spring-service> --mime application/pdf[|image/png] [--route /documents] [--ext .pdf] [--max-size 10MB] [--field document] [--dry-run] [--force] [--json]
```

## Flags

`--service`, `--mime`, `--route`, `--ext`, `--max-size`, `--field`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso make:files documents --service hr-service --mime application/pdf --ext .pdf --max-size 5MB --dry-run
pso make:files documents --service hr-service --mime application/pdf
```

## What it changes

- Writes `<Name>FilesController` (upload, download, metadata, list, move, delete on `PsoStorageService`, one scope per operation) and a test that exercises real multipart, ownership denial and deletion.

## Failure cases

- `--mime` is mandatory and must be a type the framework can verify by signature: it never generates a controller that accepts any file. The service must already use storage (`pso storage:enable`). Existing files are conflicts unless you pass --force.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [spring/storage](spring/storage.md)
- [orchestrator/file-uploads](orchestrator/file-uploads.md)
