# pso make:config

> Generate a configuration artifact. Orchestrator: a plain module in src/.app/configs/ (0.1: src/configs/) used by importing it; Spring: a @Configuration class (layered: infrastructure/config)

Generate a configuration artifact. Orchestrator: a plain module in src/.app/configs/ (0.1: src/configs/) used by importing it; Spring: a @Configuration class (layered: infrastructure/config).

## Usage

```text
pso make:config <name> [--service <name>] [--dry-run] [--force] [--json]
```

## Flags

`--service`, `--dry-run`, `--force`, `--json`.

## Examples

```bash
pso make:config storage --service gateway --dry-run
pso make:config storage --service gateway
pso make:config cache --service catalog   # Spring Boot form: a @Configuration class
```

## What it changes

- Orchestrator: writes `src/.app/configs/storage.config.ts`, a plain module whose values are read once when it is first imported. It is used by importing it (from a module controller: `import storageConfig from "../../../.app/configs/storage.config"`); the framework neither loads nor injects it.
- Spring Boot (layered): writes `infrastructure/config/CacheConfig.java`, a `@Configuration` class found by Spring's component scan like any bean of the service.

## Verify the result

- Framework settings (limits, security, logging, dispatch) are not set here: they live in `pso.config.json` (`pso config:list`).

## Failure cases

- Generation does not supply secrets: read them from the environment in the generated file, never write them into it.

## Exit codes

`0` on success, `1` when the command ran and failed. See the [exit code table](cli/index.md#exit-codes).

## See also

- [concepts/configuration](concepts/configuration.md)
- [orchestrator/services-and-providers](orchestrator/services-and-providers.md)
