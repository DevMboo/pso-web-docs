# PSO Web — Cloud Ecosystem Framework

**One ecosystem. Independent teams. Unified architecture.**

**Build independently. Integrate seamlessly. Operate securely.**

Official documentation: <https://devmboo.github.io/pso-web-docs/>

This public repository contains only the approved static portal in `docs-site/`, a standalone validation script, and the GitHub Pages workflow. It does not build from or fetch the private framework repository.

## Update the documentation

On the authorized PSO Web development machine, compile the existing portal and export it through the local publication gate:

```powershell
npx tsc -b packages/cli
node scripts/publish-docs.mjs --out publication-work/<new-run> --repo publication-work/docs-repo --denylist <private-indicator-file>
```

The script builds the portal into an isolated directory, runs the PSD security scanner with the maintainer's private indicator list, checks static file types and internal links, and copies only the validated `docs-site/` tree into this checkout. It fails if the output directory is not empty, the private indicator list is missing, or the checkout's origin is not `DevMboo/pso-web-docs`. Keep the indicator list outside both repositories. Review `security-scan.json` and the diff before committing.

```powershell
git -C publication-work/docs-repo add docs-site scripts/validate-site.mjs
git -C publication-work/docs-repo commit -m "Update public documentation"
git -C publication-work/docs-repo push origin main
```

The Pages workflow validates the static artifact again, deploys it after a push to `main`, and supports manual `workflow_dispatch`. Its deployment job reports the assigned URL. Check the completed run and the live site before announcing an update. Never copy the development repository, installer, release artifacts, assessment reports or private keys into this checkout.

The installer remains unavailable: **Private Developer Preview — Security validation in progress.** See [Downloads](https://devmboo.github.io/pso-web-docs/downloads/index.html).
