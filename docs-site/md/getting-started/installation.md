# Installation

> Use the portable Windows distribution or an authorized framework checkout, install matching Java dependencies, verify files, update and roll back.

> Status: PARTIALLY implemented — read the limitations.

> [!NOTE]
> The installed distribution on the validation machine is `0.2.0-rc.2`; its CLI reports version `0.2.0`. That candidate is not yet published in the private releases repository. [Downloads](downloads/index.md) identifies the older, published alpha.2 portable preview. Do not substitute its files for rc.2.

## Option 1: an authorized portable Windows distribution

The portable ZIP is the supported form for the current release candidate while the Windows installer is unsigned and unvalidated end to end. It needs Windows 10/11 x64, but bundles a private Node.js runtime: you do **not** install Node.js system-wide. Obtain the ZIP and its matching `SHA256SUMS`, detached signature and public key from an authorized release channel. Confirm the ZIP's SHA-256 before extraction; see [Downloads](downloads/index.md) for published assets and security limits.

1. Extract to a writable folder such as `%LOCALAPPDATA%\PSO` or `C:\Tools\pso`. Avoid `Program Files`; no administrator rights are needed.
2. Run `pso.cmd` from the extracted folder and check the installation:

   ```powershell
   .\pso.cmd --version
   .\pso.cmd doctor
   .\pso.cmd integrity check --full
   ```

3. Optionally add that folder's `bin` directory to your **user** PATH, open a new terminal, and check `Get-Command pso -All`. A development `npm link` can otherwise take precedence over the installed command.

The ZIP writes no registry entries and does not alter a system Node.js installation. The bundled `node.exe` is Authenticode-signed by the OpenJS Foundation; the PSO ZIP and scripts are **not** publisher-authenticated. Do not disable or bypass Windows or organizational application-control protections. See [Release integrity and signing](infrastructure/release-integrity-and-signing.md).

### Matching Java artifacts

For generated Spring services, use JDK 21 or newer and run `pso java:install`. The installed rc.2 distribution contains its matching Maven file repository and generates services that request `com.pso:*:0.2.0-rc.2`; `java:install` copies those artifacts into your local Maven repository. It does not publish them to Maven Central. Then build a generated service with its Maven Wrapper:

```powershell
pso java:install
cd .\catalog
.\mvnw.cmd test
```

If your authorized distribution provides the Maven file repository separately, use `pso java:install --from <maven-repository-dir>`. The source-checkout workflow below uses its own Java version; do not mix its SNAPSHOT artifacts with a release candidate. See [Maven BOM and versions](spring/maven-bom.md).

### Maintain and update

Run maintenance commands from the installed CLI, with `--root <folder>` when you are managing another installation root:

```powershell
pso integrity check --full
pso repair --full
pso update install --package <authorized-newer.psopkg>
pso update rollback
```

`integrity check` is read-only. Repair restores the installed version from its authenticated package. Update verifies a newer signed package and keeps the prior version for rollback; it does not fetch an unreleased candidate or change your workspaces. If an update changes the Java artifacts, run `pso java:install` for the newly active release. A package signed by another key is refused. See [Version migration](reference/migration.md) before adopting new project structure or infrastructure features.

## Option 2: an authorized framework checkout

For framework development, install Node.js 24 or newer and build the checkout. The `@pso/*` npm packages are not published; `npm link` points to the checkout, and moving it breaks the command:

```powershell
cd path\to\pso-web-framework
npm install
npm run build
cd packages\cli
npm link
pso doctor
```

Alternatively run `node path/to/pso-web-framework/packages/cli/bin/pso.js` without linking. A checkout also has `pso-java/`; there `pso java:install` builds and installs its `0.1.0-SNAPSHOT` artifacts, which are distinct from the installed rc.2 artifacts. Rebuild the checkout after source changes, and remove the global link with `npm unlink -g @pso/cli` when finished. [Installed versus development CLI](devops/installed-and-development-cli.md) explains how to identify which `pso` is running.

## Start a workspace

```powershell
pso create workspace shop
cd shop
pso create service catalog
pso create orchestrator gateway
pso validate
```

New projects use the [layered Java service](architecture/java-microservice.md) and [modular TypeScript orchestrator](architecture/typescript-orchestrator.md), with a functional Product CRUD example. Add `--template empty` to a `create service` or `create orchestrator` command for the same structure without the example. Existing 0.1 projects keep their legacy structure; use [Structure compatibility and migration](architecture/structure-migration.md) to plan any change.
