# pso java:install

> Install the PSO Java artifacts (BOM, starters) into the local Maven repository so generated Spring services resolve them: built from the framework checkout, or copied from a Maven file repository of a release (--from, or the one shipped with an installed CLI) without any source

Install the PSO Java artifacts (BOM, starters) into the local Maven repository so generated Spring services resolve them: built from the framework checkout, or copied from a Maven file repository of a release (--from, or the one shipped with an installed CLI) without any source.

## Usage

```text
pso java:install [--from <maven-repository-dir>] [--dry-run] [--json]
```

## Flags

`--from`, `--dry-run`, `--json`.

## Examples

```bash
pso java:install
pso java:install --from D:\releases\pso-0.1.0\maven\repository
pso java:install --from ./maven/repository --dry-run
```

## What it changes

- From a framework checkout: builds the PSO Java artifacts (BOM, starters) from `pso-java/` with the bundled Maven Wrapper and installs them into `~/.m2`.
- With `--from` (or from a release package that ships a Maven repository next to the CLI): copies prebuilt artifacts into `~/.m2` without any source or Maven build.

## Verify the result

- `pso create service <name>` no longer prints the note about missing artifacts; `./mvnw -o test` in a generated service resolves `com.pso` offline.

## Failure cases

- A local Maven or JDK failure stops installation; generated services cannot resolve unpublished `com.pso` artifacts until this succeeds.
- `No PSO Java artifacts to install`: the CLI has neither the sources nor a shipped repository; pass `--from <maven-repository-dir>` of a release.
- `is not a Maven repository with the PSO artifacts`: the directory has no `com/pso/pso-bom/<version>/`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [spring/maven-bom](spring/maven-bom.md)
- [getting-started/installation](getting-started/installation.md)
