# Task: configure Docker

> Exact steps to generate and verify container files.

1. `pso docker:init --dry-run`, then `pso docker:init`. It generates per-service `Dockerfile` and `.dockerignore`, `docker-compose.yml`, `.env.example` and `.github/workflows/ci.yml`. Existing differing files are conflicts; do not use `--force` without consent.
2. Copy `.env.example` to `.env` **locally**, fill in the real `PSO_JWT_*` values (ask the user), and never commit `.env`.
3. Check the file without Docker running: `docker compose config --quiet` (with the variables set).
4. Building and running needs a Docker daemon: `pso docker:build`, `docker compose up`. If the daemon is not running, say so; do not claim the images build.
5. The orchestrator's Dockerfile needs `@pso/core` and `@pso/http` to be installable from a registry or vendored: generated projects reference a local checkout by absolute path, which is outside the build context.
6. Preserve the generated hardening: non-root users, `read_only`, `cap_drop: ALL`, a single published port bound to `127.0.0.1`, no secrets in images. Do not publish service ports.
