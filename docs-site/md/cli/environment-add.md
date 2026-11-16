# pso environment:add

> Write deploy/<environment>.deployment.pso.ts and a pipeline declaration for it (no credentials; references only). Never overwrites an existing file without --force

Write deploy/<environment>.deployment.pso.ts and a pipeline declaration for it (no credentials; references only). Never overwrites an existing file without --force.

## Usage

```text
pso environment:add <environment> --connection <name> --repo <git-url> [--branch <branch>] [--tag <tag>] [--commit <sha>] [--runtime node|java] [--service <name>] [--start "node dist/main.js"] [--build "npm ci"] [--domain <host>] [--http-port 80] [--strategy blue-green|rolling|recreate] [--force] [--dry-run] [--json]
```

## Flags

`--connection`, `--repo`, `--branch`, `--tag`, `--commit`, `--runtime`, `--service`, `--start`, `--build`, `--domain`, `--http-port`, `--strategy`, `--force`, `--dry-run`, `--json`.

## Examples

```bash
pso environment:add production --connection prod --repo https://git.example.com/shop.git --domain shop.example.com
pso environment:add staging --connection stg --repo https://git.example.com/shop.git --runtime java --service catalog --dry-run
```

## What it changes

- Writes deploy/<environment>.deployment.pso.ts and deploy/<environment>.pipeline.pso.ts after validating them. No credential is written.

## Failure cases

- Existing files are kept unless `--force` is given.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [cloud/environments](cloud/environments.md)
