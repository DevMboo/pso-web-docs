# Installation

> Install the pso CLI from a framework checkout, as a global command or run it in place; what changes on your machine, how to verify it, how to update and remove it, and the Windows installer.

> Status: PARTIALLY implemented — read the limitations.

> [!NOTE]
> **Private Developer Preview — Security validation in progress.** The protected Windows x64 installer is not available for download. See [Downloads](downloads/index.md) for requirements, recipient enrollment and release verification. The checkout instructions below apply only to developers who already have authorized source access.

> [!WARNING]
> The `@pso/*` packages are **not published**. `npm install -g @pso/cli` is the intended future command, but the package name has not been checked for availability and nothing was published. Today you install from a checkout, or from a signed package you build yourself ([distribution](devops/distribution-and-installation.md)).

## Option 1: from a checkout, as a global command

```powershell
cd path\to\pso-web-framework
npm install
npm run build
cd packages\cli
npm link
```

`npm link` makes `pso` a global command that points at your checkout.

### Verify

```bash
pso --help
pso doctor
```

`pso --help` starts with `PSO CLI (Path Structure Working)`, then `Usage: pso <command> [options]` and the list of implemented commands, followed by the commands that are not implemented yet (they exit with code 2).

### What this means

- The global `pso` runs the code of that checkout. If you move or delete it, the command breaks.
- After you change the framework, rebuild with `npm run build` so the global command sees the change.
- Generated orchestrators reference `@pso/core` and `@pso/http` by an absolute `file:` path to the checkout until the packages are published.
- To remove the command: `npm unlink -g @pso/cli`.

## Option 2: run it without linking

```bash
node path/to/pso-web-framework/packages/cli/bin/pso.js --help
```

Useful in CI or when you do not want a global command. Replace `pso` by that invocation in every example.

## Option 3: a signed package and the Windows installer

The repository can produce a signed `.psopkg` and an unsigned Windows installer (Inno Setup 6 or 7). These are for distributing a build to another machine, not for developing the framework. They need Node 24 on the target. A private release is encrypted for individual developers: you create your own key first (`pso-recipient init`), the installer checks that it is signed by a key you pinned, and nothing is installed otherwise. All of this is described, with the real output of the steps, in [Signed packages and installation lifecycle](devops/distribution-and-installation.md).

## Java artifacts for Spring services: `pso java:install`

A generated Spring service depends on the PSO Java artifacts (`com.pso:pso-bom` and the starters). They are not on Maven Central, so they must be in your local Maven repository **before** you build or start a Spring service. Do this once per machine (and again after upgrading the framework):

| You installed the CLI from | Command | What it does |
|---|---|---|
| a framework checkout (options 1 and 2) | `pso java:install` | builds the artifacts from `pso-java/` with the bundled Maven Wrapper and installs them into `~/.m2` (a few minutes the first time; needs a JDK 21 or newer) |
| a release package, without the sources | `pso java:install --from <maven-repository-dir>` | copies prebuilt artifacts from the Maven file repository of the release (no build, no sources); a package that ships that repository next to the CLI needs no `--from` |

```bash
pso java:install
```

```text evidence="expected"
installing PSO Java 0.1.0-SNAPSHOT from D:\pso-web-framework\pso-java
...
installed com.pso:*:0.1.0-SNAPSHOT into ~/.m2
```

Verify: `pso create service catalog` no longer prints the note `the PSO Java artifacts ... are not in your local Maven repository yet`, and `./mvnw -o test` in the service resolves `com.pso` offline. Skip this step only if the workspace has no Spring service. Without it Maven fails with `Non-resolvable import POM: com.pso:pso-bom`. Image builds do not use `~/.m2`: `pso docker:init --vendor` copies the artifacts into each service ([Docker](devops/docker.md)).

## Verify the installation with a smoke test

```bash
pso create workspace smoke-test
cd smoke-test
pso create orchestrator gateway
pso validate
```

Expected output, abridged:

```text evidence="expected"
registered "gateway" in pso.workspace.json (port 3000)
gateway: 1 resource(s), digest a78ac8b26230
OK
```

`pso validate` ends with `OK` and exit code 0.

## Update

```bash
cd path/to/pso-web-framework
git pull          # or however you obtain a new checkout
npm install
npm run build
```

Already generated projects keep working; they are plain files. Regenerating a file never overwrites yours silently (a conflict is reported and you choose `--force`).

## Troubleshooting

| Symptom | Cause and fix |
|---|---|
| `pso: command not found` after `npm link` | the npm global bin directory is not on your PATH; run `npm bin -g` (or `npm prefix -g`) and add it, then open a new terminal |
| `Cannot find module '.../dist/main.js'` | the checkout was not built; run `npm run build` in the repository root |
| the command runs old behavior | rebuild; the global command runs the compiled `dist` of the checkout |
| `EACCES` on `npm link` (macOS/Linux) | configure an npm prefix you own, or use Option 2 |
| generated project: `Cannot find module '@pso/http'` | the checkout moved or `npm install` was not run in the project; fix the `file:` path in its `package.json` and run `npm install` |

## Next steps

- [Requirements](getting-started/requirements.md)
- [Quick start](getting-started/quick-start.md)
