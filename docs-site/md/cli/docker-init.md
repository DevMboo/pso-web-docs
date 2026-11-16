# pso docker:init

> Generate Dockerfiles, docker-compose.yml, .dockerignore, .env.example and a CI workflow (--transport: TLS between the orchestrator and the services, the default, with certificates from ./tls, or plaintext as an explicit choice; --vendor: carry the PSO packages inside each service, @pso/* tarballs for Node and com.pso Maven artifacts for Spring, so the build works on any machine)

Generate Dockerfiles, docker-compose.yml, .dockerignore, .env.example and a CI workflow (--transport: TLS between the orchestrator and the services, the default, with certificates from ./tls, or plaintext as an explicit choice; --vendor: carry the PSO packages inside each service, @pso/* tarballs for Node and com.pso Maven artifacts for Spring, so the build works on any machine).

## Usage

```text
pso docker:init [--transport tls|plaintext] [--vendor] [--maven-repo <dir>] [--dry-run] [--force] [--json]
```

## Flags

`--transport`, `--vendor`, `--maven-repo`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso docker:init --dry-run
pso docker:init
pso docker:init --transport plaintext --force
```

## Failure cases

- Existing files that differ are conflicts; use `--force` to overwrite.
- A NOTE or WARNING starting with TLS means the certificates in ./tls are missing or unusable: run `pso tls:init --dev` (development) or supply your own, then `pso tls:check`.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [devops/docker](devops/docker.md)
