# pso security:audit

> Audit dependencies: npm audit for Node projects, SNAPSHOT/unpinned scan for Maven projects

Audit dependencies: npm audit for Node projects, SNAPSHOT/unpinned scan for Maven projects.

## Usage

```text
pso security:audit [--json]
```

## Flags

`--json`.

## Examples

```bash
pso security:audit
```

## Failure cases

- Maven dependencies are NOT scanned for CVEs; the command says so. It runs `npm audit` for Node services and scans POMs for SNAPSHOT versions.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [security/auditing](security/auditing.md)
