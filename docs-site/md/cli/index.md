# CLI overview

> Every implemented pso command, with usage, flags, examples and failure cases.

The `pso` CLI is how you create, validate, run, secure, build and document a PSO workspace. This reference is generated from the command registry of the installed CLI, so every command listed here exists and nothing else does.

Start with [the CLI workflow](cli/workflow.md) to see which commands write files, which inspect declarations, and which contact running processes. Return to a command page for its exact usage and failure cases.

> [!TIP]
> Every command supports `--help`. Most support `--json` for machine output, and every command that writes files supports `--dry-run`.

## Exit codes

| Code | Meaning |
|---|---|
| 0 | Success |
| 1 | The command ran and failed: validation errors, conflicts, a refused unsafe request |
| 2 | Unknown command, or a command from the specification that is not implemented |
| 3 | Explicit authorization required (`--yes`) for `deploy` or `rollback` |
| 70 | Internal error |

## Commands

| Command | Summary |
|---|---|
| [`pso agent:enroll`](cli/agent-enroll.md) | Enroll this machine with a single-use token an administrator created (the token is read from standard input or a hidden prompt, never from an argument) |
| [`pso agent:install`](cli/agent-install.md) | Install the runtime agent over SSH, provision it (server identity, policy, a single-use administrator enrollment) and enroll this machine as its first superadministrator. Remote management stays disabled unless you choose a mode |
| [`pso agent:releases`](cli/agent-releases.md) | List the agent releases kept on the server: the active and the previous one, each build's version, build id, checksum, installation time and installer, and the history of activations and rollbacks |
| [`pso agent:rollback`](cli/agent-rollback.md) | Return the server's agent to an earlier release (the previous one, or a named one from agent:releases): the release's checksum is verified again and it must start before the link is switched atomically. Server identity, roles, policy and deployments are untouched |
| [`pso agent:status`](cli/agent-status.md) | Show the agent's version, mode and who is enrolled |
| [`pso agent:uninstall`](cli/agent-uninstall.md) | Remove the management agent from the server. Applications, their data and processes stay unless --purge-applications is given (refused while services run) |
| [`pso agent:upgrade`](cli/agent-upgrade.md) | Install the agent bundled in this installation as a new versioned release on the server: verified upload, start check, one atomic link switch; every earlier release is kept (see agent:releases) and can be returned to with agent:rollback. Identity, roles, policy and deployments are untouched; the policy is never changed by an upgrade |
| [`pso ai:context`](cli/ai-context.md) | Generate AI-assistant context from the installed framework and the current project (never includes secrets) |
| [`pso ai:init`](cli/ai-init.md) | Inspect the workspace and create AGENTS.md, CLAUDE.md and Copilot instructions with project-specific PSO guidance |
| [`pso build`](cli/build.md) | Type-check Node services and package artifacts (npm pack / Spring Boot jar) with checksums into .pso/artifacts |
| [`pso channels:contracts`](cli/channels-contracts.md) | Generate the client-safe TypeScript contracts of the registered channels (names, parameters, event and command payloads) for a front end; internal channels, publishers, scopes and policies are never included |
| [`pso channels:enable`](cli/channels-enable.md) | Connect a service to PSO Channels through Dispatch: for a Spring service the starter, pso.channels and the channels.publish event; for the orchestrator @pso/channels and the Dispatch consumer that delivers events to browsers |
| [`pso channels:validate`](cli/channels-validate.md) | Check channel declarations without running them: invalid, duplicate or ambiguous patterns, classes registered in MODULE.pso.ts that are not channels, channel classes that are not registered |
| [`pso config:list`](cli/config-list.md) | List configuration sources found per scope and the known configuration keys |
| [`pso config:show`](cli/config-show.md) | Show the effective configuration with the origin of every value (secrets redacted) |
| [`pso config:validate`](cli/config-validate.md) | Validate configuration against the typed schema and secret-handling rules (exit 1 on errors; unknown keys are errors with --strict and in production) |
| [`pso connection:add`](cli/connection-add.md) | Register an SSH connection to a Linux server: pins its SSH host key after you approve the fingerprint, stores a key passphrase or password only in the protected credential store |
| [`pso connection:key`](cli/connection-key.md) | Diagnose the access rights of the connection's SSH private key file (OpenSSH refuses a key other users can read); with --repair --yes restrict the file to you after showing each command |
| [`pso connection:list`](cli/connection-list.md) | List registered connections (references only: no secret is ever shown) |
| [`pso connection:remove`](cli/connection-remove.md) | Delete a connection from this machine (credentials and pinned host key included); the server and its applications are not touched |
| [`pso connection:revoke`](cli/connection-revoke.md) | Revoke this machine's access: the server forgets the client identity and every local secret of the connection is deleted |
| [`pso connection:rotate`](cli/connection-rotate.md) | Rotate credentials: by default the Ed25519 client key used with the agent (enroll a new key, revoke the old); with --ssh-secret the stored SSH password or key passphrase (new value on stdin) |
| [`pso connection:test`](cli/connection-test.md) | Check a connection layer by layer: SSH and host identity, the agent, the agent's pinned identity, your authenticated session |
| [`pso connection:tunnel`](cli/connection-tunnel.md) | Open a private SSH tunnel from a loopback port here to a loopback port on the server (for example a service's actuator); never binds publicly, runs until Ctrl+C |
| [`pso create`](cli/create.md) | Create a workspace, an orchestrator, a Spring Boot service, or a frontend project: the application shell (fei) or a microfrontend (mei) |
| [`pso crud:bind`](cli/crud-bind.md) | Bind a CRUD module of the orchestrator to the microservice that serves its resource (the <R>_SERVICE contract constant and the metadata.services of its routes, together) |
| [`pso database:enable`](cli/database-enable.md) | Opt a Spring service into a database: adds the persistence starter and sets pso.database.provider (H2 only for the local profile) |
| [`pso deploy`](cli/deploy.md) | Apply a deployment (requires --yes); verifies checksums and health, rolls back on failed verification. --env deploys an environment of pso.infra.json (Docker target: build, record, start, verify through the edge, automatic rollback; bundle target: a reviewable bundle for the operator); --dry-run only plans |
| [`pso deploy:init`](cli/deploy-init.md) | Generate hardened deployment descriptors for Kubernetes, AWS ECS (Fargate), Azure Container Apps or Google Cloud Run (nothing is ever applied to a cloud) |
| [`pso deploy:logs`](cli/deploy-logs.md) | Show a container's log from the environment's host (secrets, tokens and keys masked) |
| [`pso deploy:plan`](cli/deploy-plan.md) | Show exactly what a deployment would change, without changing anything (--env: the 0.2 environment deployment of pso.infra.json) |
| [`pso deploy:status`](cli/deploy-status.md) | Show the active release of an environment, its recorded releases and history, and the state and health of its containers (on the remote host for an SSH target, on this machine for the docker target) |
| [`pso deploy:unlock`](cli/deploy-unlock.md) | Remove the deployment lock an interrupted deployment left on the environment's host (only when no deployment is running) |
| [`pso deploy:validate`](cli/deploy-validate.md) | Statically check the deployment descriptors (security posture, probes, limits, inline secrets, unfilled placeholders); no cloud is contacted |
| [`pso dev`](cli/dev.md) | Validate, start, monitor and gracefully stop the workspace (Ctrl+C stops). Local processes by default; --docker runs the environment's containers instead. --service starts one service (and with --with-dependencies what it depends on: other services, its database and the identity provider), --fei also starts that frontend's development servers |
| [`pso dev:env`](cli/dev-env.md) | Show where every variable of a service comes from when `pso dev` starts it (its files, the environment of pso dev, the workspace, the infrastructure environment) and which infrastructure settings are not applied and why. Names and sources only: no value is ever shown, secrets are only reported as resolved |
| [`pso dispatch:consumers`](cli/dispatch-consumers.md) | List every declared consumer (event, handler, service, group) |
| [`pso dispatch:doctor`](cli/dispatch-doctor.md) | Validate and then really connect to each configured broker; reports up, down or not configured (never claims a connection from configuration alone) |
| [`pso dispatch:events`](cli/dispatch-events.md) | List every declared event with its version, channel, owner service, publishers and consumers |
| [`pso dispatch:graph`](cli/dispatch-graph.md) | Show the declared event topology: publishers -> channels -> consumers (declarations, not live broker state) |
| [`pso dispatch:inspect`](cli/dispatch-inspect.md) | Show the declared dispatch setup of each service: provider, enabled state, events, consumers and settings (secrets redacted; no broker is contacted) |
| [`pso dispatch:install`](cli/dispatch-install.md) | Install a dispatch provider into a Spring service or the orchestrator (dependency, configuration, local broker file). Dispatch stays disabled until you enable it |
| [`pso dispatch:lock`](cli/dispatch-lock.md) | Record the current event contracts in pso.dispatch.lock.json so later schema changes are checked for compatibility |
| [`pso dispatch:outbox`](cli/dispatch-outbox.md) | Enable the transactional outbox in a Spring service with PostgreSQL: adds the pso_outbox Flyway migration and sets pso.dispatch.outbox.enabled (idempotent) |
| [`pso dispatch:providers`](cli/dispatch-providers.md) | List the supported dispatch providers, their guarantees and which service has which adapter installed |
| [`pso dispatch:remove`](cli/dispatch-remove.md) | Remove the installed dispatch provider from a service and disable dispatch (event declarations and handlers are never deleted) |
| [`pso dispatch:validate`](cli/dispatch-validate.md) | Statically validate event declarations, schemas, consumers, providers and the contract lock (exit 1 on errors) |
| [`pso dist`](cli/dist.md) | Build, sign, encrypt, verify and extract signed .psopkg distribution packages from an explicit allowlist |
| [`pso docker:build`](cli/docker-build.md) | Build the service images. With pso.infra.json: for the environment's generated topology (Dockerfiles are created when missing, never overwritten; --vendor copies the framework packages into the services). Without it: the 0.1 behaviour (docker-compose.yml from pso docker:init) |
| [`pso docker:down`](cli/docker-down.md) | Stop and remove the environment's containers and networks. Persistent data (database volumes) is kept; --volumes deletes it too and needs --yes |
| [`pso docker:init`](cli/docker-init.md) | Generate Dockerfiles, docker-compose.yml, .dockerignore, .env.example and a CI workflow (--transport: TLS between the orchestrator and the services, the default, with certificates from ./tls, or plaintext as an explicit choice; --vendor: carry the PSO packages inside each service, @pso/* tarballs for Node and com.pso Maven artifacts for Spring, so the build works on any machine) |
| [`pso docker:inspect`](cli/docker-inspect.md) | Show the effective definition of a container (image, environment, mounts, secrets, health check, limits, dependencies) as Docker Compose resolves it, with its runtime state |
| [`pso docker:logs`](cli/docker-logs.md) | Show a container's log (credentials, tokens and known secret values are masked) |
| [`pso docker:plan`](cli/docker-plan.md) | Show the container topology of an environment before anything runs: startup order, images built or pulled, published ports, networks, volumes, secrets (names only), health checks and resource limits |
| [`pso docker:ps`](cli/docker-ps.md) | List the environment's containers with their state and health |
| [`pso docker:restart`](cli/docker-restart.md) | Restart only the named containers (their dependencies keep running). --recreate re-creates them from the current topology, which picks up changed environment, mounts or images |
| [`pso docker:up`](cli/docker-up.md) | Start the environment's containers in dependency order and wait until every one is healthy (development: missing infrastructure is provisioned and missing images are built first). --service starts those containers and what they depend on |
| [`pso docs`](cli/docs.md) | Serve the documentation portal locally, or write the complete static site (search, llms.txt, AI context) with --build |
| [`pso doctor`](cli/doctor.md) | Check the local toolchain |
| [`pso environment:access`](cli/environment-access.md) | List who may manage the environment; --invite creates a single-use enrollment token for a teammate (shown once), --revoke ends a client's access immediately |
| [`pso environment:add`](cli/environment-add.md) | Write deploy/<environment>.deployment.pso.ts and a pipeline declaration for it (no credentials; references only). Never overwrites an existing file without --force |
| [`pso environment:inspect`](cli/environment-inspect.md) | Inspect the remote server (OS, CPU, memory, disk, ports, Java, Node, git, nginx, existing PSO apps and vhosts); read-only |
| [`pso environment:list`](cli/environment-list.md) | List the environments declared in this project (deploy/*.deployment.pso.ts) with their connection, strategy and declaration problems |
| [`pso environment:network`](cli/environment-network.md) | Every listening socket of the server, who owns it and whether it is meant to be public. Flags internal infrastructure (RabbitMQ, Redis, PostgreSQL, Kafka, PSO services) on all interfaces as potentially unsafe and prints a remediation plan; it changes nothing. --verify adds a TCP check from this machine |
| [`pso environment:plan`](cli/environment-plan.md) | Show what provisioning would do on the server: steps PSO performs unprivileged, steps an administrator must run, and the exact script for them (nothing is executed) |
| [`pso environment:policy`](cli/environment-policy.md) | Show the server-side deployment policy (mode, approvals, allowed strategies, retention...); --set changes it (superadministrator, audited). The server policy overrides anything a project declares |
| [`pso environment:privileges`](cli/environment-privileges.md) | The EFFECTIVE sudo privilege of the agent's user: none, restricted (exact commands), broad (wildcards, shells, editors), unrestricted or unknown. A privileged command that works is never read as least privilege |
| [`pso environment:provision`](cli/environment-provision.md) | Apply the unprivileged provisioning steps (agent directories, default policy). Idempotent. Privileged steps are only listed: PSO never installs packages, edits nginx.conf, changes firewall rules or replaces existing configuration |
| [`pso environment:secret`](cli/environment-secret.md) | Store a named secret on the server (value from standard input or a hidden prompt) for services to receive as environment variables; --list shows names only, never values |
| [`pso fei:register`](cli/fei-register.md) | Register a MEI with the FEI: adds it to FEI.pso.ts (mount path, entry scopes, loading) and validates the relationship |
| [`pso fei:unregister`](cli/fei-unregister.md) | Remove a MEI from FEI.pso.ts (the MEI project itself is not deleted) |
| [`pso frontend:build`](cli/frontend-build.md) | Build the FEI and/or MEIs independently (hashed assets); a MEI also gets its release manifest (file hashes, contract, shared versions, optional ed25519 signature) |
| [`pso frontend:config`](cli/frontend-config.md) | Configure how users sign in to a FEI. With --env: record the sign-in mode of that FEI in that environment of pso.infra.json and re-render its site configuration through the infrastructure engine (other FEIs and environments unchanged). With --site: write the FEI's runtime configuration on a site root you manage: where each approved MEI's release pointer is, who may sign releases, how users sign in and the orchestrator's same-origin path. It holds no secret and is never built into the bundle |
| [`pso frontend:contracts`](cli/frontend-contracts.md) | Generate typed frontend contracts from the orchestrator's real route metadata and DTO classes into every frontend project (src/models/contract.generated.ts); --check fails when a project's copy is stale |
| [`pso frontend:declare`](cli/frontend-declare.md) | Write deploy/<name>.deployment.pso.ts for a frontend project so the native deployment engine can plan, deploy, verify and roll it back (`pso release:plan`, `release:deploy`, `release:rollback`) |
| [`pso frontend:dev`](cli/frontend-dev.md) | Start the development servers: every FEI and its registered MEIs (ports from their declarations), one FEI with its MEIs (--fei), or one MEI alone with the development host adapter (--standalone) |
| [`pso frontend:graph`](cli/frontend-graph.md) | Show how the FEI, its MEIs and the orchestrator relate |
| [`pso frontend:inspect`](cli/frontend-inspect.md) | Show a frontend project: identity, port, orchestrator, registered MEIs, middleware order, scopes and effective routes |
| [`pso frontend:install`](cli/frontend-install.md) | Vendor the framework packages into the frontend projects and run `npm install` in each (or only in <project>) |
| [`pso frontend:make`](cli/frontend-make.md) | Generate a navigation middleware (FEI: src/middlewares, MEI: domain middleware) or add a route with its page to ROUTES.pso.ts |
| [`pso frontend:nginx`](cli/frontend-nginx.md) | Print a same-origin nginx configuration for a site root (FEI, remotes, API kept apart; restrictive Content-Security-Policy). The managed adapter of the runtime agent generates exactly these locations when the shell is deployed (`pso frontend:declare`), so this is a preview for review |
| [`pso frontend:publish`](cli/frontend-publish.md) | Publish a built FEI or MEI release to a site root: immutable versioned files, compatibility and integrity checks, then one atomic activation (nothing already published is overwritten or deleted) |
| [`pso frontend:releases`](cli/frontend-releases.md) | Show the published versions of each MEI and of the FEI on a site root: which is active and the activation history |
| [`pso frontend:rollback`](cli/frontend-rollback.md) | Return one MEI to its previous (or a chosen) published release, after the same compatibility checks; the other microfrontends and the FEI are not touched |
| [`pso frontend:routes`](cli/frontend-routes.md) | List the effective browser routes of the whole frontend ecosystem with their owners (exit 1 on conflicts) |
| [`pso frontend:validate`](cli/frontend-validate.md) | Validate the FEI/MEI declarations, their relationships, route ownership and conflicts, contract compatibility and the architecture rules (exit 1 on errors) |
| [`pso graph`](cli/graph.md) | Show service dependencies and orchestrator routes together |
| [`pso host:check`](cli/host-check.md) | Check that the host an environment deploys to (deploy.target "ssh") meets its prerequisites: Docker and Compose, a writable root with free space, the edge port, and every operator-supplied certificate (verified: authority, names, expiry; keys present) and secret file (present; never read). Exit 1 when something blocks a deployment |
| [`pso host:inspect`](cli/host-inspect.md) | Inspect a registered Linux host over SSH (read-only): operating system and architecture, Docker and Compose available to the deploy user, free disk under the deployment root, listening ports, existing PSO-managed deployments and registry reachability |
| [`pso idp:dev`](cli/idp-dev.md) | DEVELOPMENT identity provider for a workspace: init generates a signing key on this machine, its public key set, a JWKS server and a Compose overlay; token signs a development JWT; serve publishes the keys on loopback for `pso dev`. Never for production (refused with PSO_ENV=production) |
| [`pso infra:apply`](cli/infra-apply.md) | Provision the environment idempotently: create what is missing and PSO may generate (development only), update configuration whose declarations changed, keep everything valid, never touch operator-supplied resources or hand edits |
| [`pso infra:hosts`](cli/infra-hosts.md) | Show the hosts entries a development environment needs; with --apply (or --remove) and --yes write (or remove) PSO's marked block in the hosts file, nothing else (the system file needs administrator rights). Browsers resolve *.localhost without it |
| [`pso infra:identity`](cli/infra-identity.md) | Reconcile the environment's running Keycloak realm with the declarations through its administration API: FEI clients, redirect URIs, web origins, the service client, scope roles, mappers and development users. Without --apply it reports drift; changing an existing managed client or removing one (--prune) needs --yes. Users, credentials, sessions, client secrets and resources PSO did not create are never changed |
| [`pso infra:init`](cli/infra-init.md) | Write pso.infra.json, the declarative infrastructure contract (environments, hosts, TLS, secrets, identity provider), derived from the workspace: one local development environment to start with |
| [`pso infra:inspect`](cli/infra-inspect.md) | Show the resolved topology of an environment (hosts, origins, OIDC redirect URIs, addresses as containers and as processes see them) and its resources with owner, files and facts (fingerprints, expiry; never key material or secret values) |
| [`pso infra:plan`](cli/infra-plan.md) | Show what the infrastructure of an environment needs and what applying would do (create, replace, update, supply), without changing anything |
| [`pso infra:repair`](cli/infra-repair.md) | Resolve drift and damage: regenerate managed files that were edited by hand (the edited copy is kept in .infra/backups/), complete interrupted provisioning, restrict exposed key files. Needs --yes when something would be overwritten |
| [`pso infra:rotate`](cli/infra-rotate.md) | Replace a valid certificate, authority, signing key or secret on purpose (rotation). Replacing the authority re-issues every certificate it signed. Development environments only: production material is rotated by its owner |
| [`pso infra:status`](cli/infra-status.md) | One-screen health of an environment's infrastructure: resources by status, drift, certificates close to expiry, the last provisioning and whether one was interrupted |
| [`pso infra:token`](cli/infra-token.md) | Get an access token for a development user: signed offline (pso-dev identity) or issued by the environment's Keycloak (password grant through the development-only API client). Development environments only |
| [`pso infra:validate`](cli/infra-validate.md) | Validate an environment's infrastructure: integrity of authorities, certificates and keys, expiry, permissions, drift, ignore rules, secrets leaked into files, and with --probe the running environment (TLS, security headers, API readiness, identity provider) for real (exit 1 on errors) |
| [`pso init`](cli/init.md) | Create pso.workspace.json in the current directory from the Node/Spring projects found in it |
| [`pso inspect`](cli/inspect.md) | List the resources PSO discovered in a project |
| [`pso install:react`](cli/install-react.md) | Install an official PSO integration into an existing react application: detects the project and its package manager, installs the packages, writes small application-owned files, never overwrites yours, then type-checks the result |
| [`pso integrity`](cli/integrity.md) | Verify the installed files against the signed inventory (read-only: nothing is ever modified) |
| [`pso java:install`](cli/java-install.md) | Install the PSO Java artifacts (BOM, starters) into the local Maven repository so generated Spring services resolve them: built from the framework checkout, or copied from a Maven file repository of a release (--from, or the one shipped with an installed CLI) without any source |
| [`pso lint`](cli/lint.md) | Static checks: workspace validation, configuration, security check (for one environment, with the sources the application runs with in it) and TypeScript type-check |
| [`pso make:adapter`](cli/make-adapter.md) | Generate an adapter component (Spring, 0.1 hexagonal layout only: adapters/; layered services use clients: pso make:client) |
| [`pso make:channel`](cli/make-channel.md) | Generate a channel class (and, for a resource channel, a deny-by-default access provider and the event contract), register it in MODULE.pso.ts and add @pso/channels |
| [`pso make:client`](cli/make-client.md) | Generate an outbound client in infrastructure/client (layered Spring service) that calls a declared service through PsoHttpClient |
| [`pso make:config`](cli/make-config.md) | Generate a configuration artifact. Orchestrator: a plain module in src/.app/configs/ (0.1: src/configs/) used by importing it; Spring: a @Configuration class (layered: infrastructure/config) |
| [`pso make:contract`](cli/make-contract.md) | Generate a contract. With a module (modular orchestrator): TypeScript types of a microservice API in modules/<module>/contracts/; without: a JSON service contract (src/.app/contracts/, 0.1: src/contracts/) |
| [`pso make:controller`](cli/make-controller.md) | Generate a controller (orchestrator: inside a module; Spring: hexagonal layout) |
| [`pso make:crud`](cli/make-crud.md) | Generate a complete CRUD resource in the official structures: in a Spring service (entity, Flyway migration, repository with database-level filters, request/response records, mapper, transactional service, controller with scopes, tests) and/or, with --orchestrator, the matching orchestrator module (DTOs, contract, provider, service, mapper, controller, routes) bound to that service |
| [`pso make:decorator`](cli/make-decorator.md) | Generate a reusable method decorator in src/.app/decorators/ (modular orchestrator): application metadata, never routing or security |
| [`pso make:dispatch-event`](cli/make-dispatch-event.md) | Declare a DISTRIBUTED PSO Dispatch event and its payload contract. Spring: dispatch/*.events.json and a payload record; orchestrator: src/dispatch/<module>/EVENTS.pso.ts and a schema. --consume-only declares an event another service publishes |
| [`pso make:dispatch-handler`](cli/make-dispatch-handler.md) | Generate a declared consumer of a PSO Dispatch event that this project already declares. Spring: a @PsoEventHandler component; orchestrator: a @DispatchConsumer class listed in EVENTS.pso.ts |
| [`pso make:dto`](cli/make-dto.md) | Generate a dto (orchestrator: inside a module; Spring: hexagonal layout) |
| [`pso make:entity`](cli/make-entity.md) | Generate a JPA entity in infrastructure/entity (layered Spring service); add its table with pso make:migration |
| [`pso make:event`](cli/make-event.md) | Generate a LOCAL Spring application event (a record published with ApplicationEventPublisher and delivered in the same process). For an event between services use make:dispatch-event |
| [`pso make:exception`](cli/make-exception.md) | Generate an application exception in shared/exception (layered Spring service), answered as an RFC 9457 problem |
| [`pso make:files`](cli/make-files.md) | Generate a Spring files controller (upload, download, metadata, list, move, delete on PsoStorageService with scope checks) and its test |
| [`pso make:filter`](cli/make-filter.md) | Generate an error filter. Orchestrator: src/.app/filters/ (0.1: src/filters/), run for the routes that name it; Spring (layered): a servlet filter in infrastructure/filter |
| [`pso make:guard`](cli/make-guard.md) | Generate a guard (orchestrator: inside a module; Spring: hexagonal layout) |
| [`pso make:hook`](cli/make-hook.md) | Generate a lifecycle hook (onReady / onShutdown) in the module's `hooks/` folder (`src/modules/<module>/hooks/`, `--module` is required in the official structure) and list it in that module's providers (a hook nobody lists never runs); a 0.1 project writes `src/hooks/` |
| [`pso make:interceptor`](cli/make-interceptor.md) | Generate an interceptor in src/.app/interceptors/ (modular orchestrator). It runs around the action of the routes that name it (interceptors: [...] in ROUTES.pso.ts) |
| [`pso make:listener`](cli/make-listener.md) | Generate a LOCAL Spring @EventListener for the application event of the same name (same process). For an event from another service use make:dispatch-handler |
| [`pso make:mapper`](cli/make-mapper.md) | Generate a mapper. Orchestrator (modular): a mapping function in modules/<module>/mappers/; Spring: a mapper component (layered: application/mapper) |
| [`pso make:middleware`](cli/make-middleware.md) | Generate a middleware in src/.app/middlewares/ (orchestrator; 0.1: src/middlewares/). It runs only for the routes that name it in ROUTES.pso.ts |
| [`pso make:migration`](cli/make-migration.md) | Create the next Flyway migration (V<n>__<name>.sql) in a PostgreSQL-backed service |
| [`pso make:module`](cli/make-module.md) | Create an HTTP module: MODULE.pso.ts + an empty ROUTES.pso.ts (no endpoint is exposed yet) |
| [`pso make:notifier`](cli/make-notifier.md) | Generate a typed Spring publisher for a channel event (a record and a service on PsoChannelPublisher; --durable stores the event in the transactional outbox) |
| [`pso make:port`](cli/make-port.md) | Generate a port interface (Spring, 0.1 hexagonal layout only: ports/; the layered structure has no ports) |
| [`pso make:provider`](cli/make-provider.md) | Generate an injectable provider class in the module's `providers/` folder (`src/modules/<module>/providers/`, `--module` is required in the official structure) and list it in that module's providers (explicit registration: a provider nobody lists is never constructed); a 0.1 project writes `src/providers/` |
| [`pso make:repository`](cli/make-repository.md) | Generate a Spring Data repository (layered: infrastructure/persistence, for an existing entity; 0.1: persistence/). It does not enable a database: see `pso database:enable` |
| [`pso make:request`](cli/make-request.md) | Generate a request record with Jakarta Validation in application/dto/request (layered Spring service) |
| [`pso make:resource`](cli/make-resource.md) | Generate a connected route + contract in the orchestrator and controller + DTO + test in a Spring service |
| [`pso make:response`](cli/make-response.md) | Generate a response record in application/dto/response (layered Spring service) |
| [`pso make:route`](cli/make-route.md) | Add a decorated controller action AND its ROUTES.pso.ts declaration, kept consistent (rolled back if validation fails). With --upstream the action forwards the request to one service; with --compose it gets the UpstreamClient and the route declares the services it may call to build its response. With --upstream and --dto the body is validated at the gateway and then forwarded unchanged; a DTO with file fields generates an upload (forwardUpload); --download a streamed file download (forwardDownload) |
| [`pso make:service`](cli/make-service.md) | Generate a service (orchestrator: inside a module; Spring: hexagonal layout) |
| [`pso migrate`](cli/migrate.md) | Bring files generated by an earlier PSO version up to date without touching your own code: shows what would change (the fixed 10 s shutdown timer of older orchestrators becomes shutdownOnSignals); --apply writes it and keeps the previous file as <file>.bak |
| [`pso pipeline:validate`](cli/pipeline-validate.md) | Statically validate deployment and pipeline declarations (no network, no code executed): structure, secrets in declarations, strategy feasibility, stage order and dependencies |
| [`pso release:approve`](cli/release-approve.md) | Approve a proposed plan (the server policy decides whether the proposer may approve their own plan). The approval is bound to the plan hash, the commit and the policy |
| [`pso release:audit`](cli/release-audit.md) | The server's audit log (sanitized; hash-chained, so edits are detectable by --verify). It is evidence against accidents, not against a fully privileged server administrator |
| [`pso release:cancel`](cli/release-cancel.md) | Cancel a running deployment at the next safe point (before traffic moves); the active release is untouched. After traffic moved, use rollback |
| [`pso release:config`](cli/release-config.md) | The effective configuration of the ACTIVE release as the server holds it: services, ports, routes, health, environment variables (values that look like secrets are redacted) and secret reference names. Never a secret value. |
| [`pso release:deploy`](cli/release-deploy.md) | Deploy: with --plan, exactly that approved plan; without it, a fresh plan is made, shown and executed. The deployment runs on the server and continues if this command or your connection ends |
| [`pso release:endpoints`](cli/release-endpoints.md) | The routes published WITH the active release (method, effective URL, controller, service, guard, required scopes), from release metadata; a release without metadata says so. Declared, deployed and verified are different states: nothing here is 'verified' until a request proves it |
| [`pso release:history`](cli/release-history.md) | Deployment history from the server (newest first): who, what, strategy, outcome |
| [`pso release:logs`](cli/release-logs.md) | Sanitized logs from the server: a deployment's log (default: the latest) or the active release's application log of a service. Secrets known to the server are masked |
| [`pso release:nginx`](cli/release-nginx.md) | The real nginx configuration of the server: files, enabled links, effective includes, virtual hosts, upstreams, TLS paths and which files PSO manages. Edits are limited to PSO-managed files, are linted, checked against concurrent changes, validated by nginx -t and restored automatically on failure |
| [`pso release:plan`](cli/release-plan.md) | Ask the server for a deployment plan: resolves the branch or tag to an immutable commit, checks resources, strategy, nginx, policy and pipeline. Changes nothing but the stored proposal |
| [`pso release:recover`](cli/release-recover.md) | Reconcile an interrupted deployment: inspects what really exists on the server, then completes it, undoes it, or stops and asks for an operator when the state is ambiguous. Never replays steps blindly |
| [`pso release:rollback`](cli/release-rollback.md) | Re-activate the previous (or a chosen kept) release with the same candidate-verify-switch-drain sequence. Application rollback does not undo database changes |
| [`pso release:service`](cli/release-service.md) | Inspect, restart, start or stop ONE service of the active release without touching the others (not allowed while a deployment is running) |
| [`pso release:sign`](cli/release-sign.md) | Sign the release manifest (application, immutable commit or artifact checksum, strictly increasing sequence) with a local signer. The server verifies it against its own trust store and refuses replays and downgrades |
| [`pso release:status`](cli/release-status.md) | Active, previous and last known-good release, the lock, the services and the latest (or chosen) deployment as the server sees it (INTERRUPTED when its worker is gone) |
| [`pso release:url`](cli/release-url.md) | Where the deployed application can be reached, from the deployment metadata and the REAL nginx file; --verify measures each layer independently (process, local health, proxy, this machine, TLS, optional SSH tunnel). COMPLETED is not proof of public access |
| [`pso release:verify`](cli/release-verify.md) | Ask the environment's SERVER whether a signed manifest is authentic (a trusted, unrevoked signer) and integral (it names exactly the release), reporting the two separately |
| [`pso repair`](cli/repair.md) | Restore damaged or missing files of the installed version from an authenticated package (never upgrades; user data is never touched) |
| [`pso rollback`](cli/rollback.md) | Re-activate the previous release of a native deployment, or (--env) of an environment deployed to Docker |
| [`pso routes:inspect`](cli/routes-inspect.md) | Show one orchestrator route in full: target, access policy, contract and the service-side handler |
| [`pso routes:list`](cli/routes-list.md) | List the routes declared in ROUTES.pso.ts files with their effective policy; fails when the registry is invalid |
| [`pso security:audit`](cli/security-audit.md) | Audit dependencies: npm audit for Node projects, SNAPSHOT/unpinned scan for Maven projects |
| [`pso security:check`](cli/security-check.md) | Examine real configuration and sources for security problems, for one environment and with the configuration sources the application runs with in it (exit 1 on high/critical; --strict also medium) |
| [`pso security:keys`](cli/security-keys.md) | List signing keys (public data only) or generate the first keypair |
| [`pso security:policy`](cli/security-policy.md) | Show the effective authorization policy of every route and service |
| [`pso security:rotate`](cli/security-rotate.md) | Create a new active key; the previous one stays in the JWKS as 'retiring' until retired |
| [`pso security:validate`](cli/security-validate.md) | Validate the security configuration of the orchestrator (security.authentication.*) and of every PSO Spring service (pso.security.*) before anything runs |
| [`pso services:graph`](cli/services-graph.md) | Show the service dependency graph and startup order |
| [`pso services:health`](cli/services-health.md) | Probe every declared service live (exit 1 if any is DOWN) |
| [`pso services:inspect`](cli/services-inspect.md) | Show declaration, discovered resources and live health of one service |
| [`pso services:list`](cli/services-list.md) | List services declared in the workspace |
| [`pso services:register`](cli/services-register.md) | Declare an existing project as a workspace service (validated before saving) |
| [`pso services:unregister`](cli/services-unregister.md) | Remove a service declaration (refused while others depend on it; files are never deleted) |
| [`pso setup`](cli/setup.md) | Install a signed .psopkg into an installation root (verified completely before anything is written; side by side with other versions) |
| [`pso signer:generate`](cli/signer-generate.md) | Create an Ed25519 release-signing key on THIS machine. The private key goes to the protected credential store (never to a file, a project or a server); the public key is what an environment is told to trust |
| [`pso signer:list`](cli/signer-list.md) | List the signing identities of this machine (names, key ids, public keys; never private keys) |
| [`pso signer:remove`](cli/signer-remove.md) | Delete a signing key from THIS machine. It does not revoke trust on servers: if the key may be compromised, run pso trust:revoke on every environment first |
| [`pso storage:enable`](cli/storage-enable.md) | Opt a Spring service into PSO Storage: adds the storage starter (and the S3 or Azure SDK), the pso.storage settings and multipart uploads; S3/Azure services use the local provider in the local and test profiles |
| [`pso storage:validate`](cli/storage-validate.md) | Validate the pso.storage.* configuration of the Spring services (provider values, secrets in files, limits that disagree, missing dependencies) before anything runs |
| [`pso structure:inspect`](cli/structure-inspect.md) | Show the workspace layout (standard, or flat with the plan to move its projects into orchestrators/, services/, frontends/ and frontends/modules/), the structural convention of each project (modular / layered / 0.1), mixed or contradictory structures, and for a 0.1 project the migration plan: every file to move, the imports, route declarations, module registrations and Java package declarations it affects. Read-only: nothing is moved |
| [`pso studio`](cli/studio.md) | Open the local developer dashboard (live services, graph, routes, config, security, logs) |
| [`pso test`](cli/test.md) | Run each service's own tests (npm test / mvnw test); fails if any fail |
| [`pso tls:check`](cli/tls-check.md) | Verify the workspace's TLS material: files, issuing authority, validity dates, private keys and the host names the orchestrator will verify (exit 1 on any problem; no key material is printed) |
| [`pso tls:init`](cli/tls-init.md) | Create DEVELOPMENT TLS certificates for the workspace's services (a local certificate authority and one certificate per service, named after the service) so a local stack runs with verified TLS. Production certificates are supplied in the same layout, never generated here |
| [`pso trust:add`](cli/trust-add.md) | Make the environment trust a signer's PUBLIC key (superadministrator, audited). A server that requires signed releases accepts only signers added here; no project file can add one |
| [`pso trust:list`](cli/trust-list.md) | Show which signers the environment's server trusts (and which were revoked or expired) |
| [`pso trust:revoke`](cli/trust-revoke.md) | Revoke a trusted signer (superadministrator, audited). A revoked key is never trusted again; releases it signed no longer qualify, including for rollback where signing is required |
| [`pso types`](cli/types.md) | Write src/pso-types.d.ts in the orchestrator: the registered service names and the guard names as TypeScript types, so the compiler checks them wherever they are named (--check: exit 1 when the file is missing or out of date) |
| [`pso uninstall`](cli/uninstall.md) | Remove an installation using its inventory: standard keeps config/cache/logs, --complete removes everything PSO owns, --purge also deletes projects you name and confirm |
| [`pso update`](cli/update.md) | Check for, install (side by side, verified, atomically activated) and roll back signed updates; offline packages are supported |
| [`pso validate`](cli/validate.md) | Validate workspace structure, conventions and contracts (exit 1 on errors) |
| [`pso workspace:migrate`](cli/workspace-migrate.md) | Migrate a workspace to 0.2.0: add pso.infra.json, adopt the existing development TLS authority, certificates and identity key (reused, not regenerated), provision the local environment, and add the PUBLIC rules that Spring services generated before 0.2.0 lack for routes the orchestrator declares public (with backups). Without --apply it only reports; application logic, declarations and data are never changed |

## Not implemented

These exist in the specification but exit with code 2 in this version: `pso dispatch:health`. See [Event dispatch](dispatch/overview.md).
