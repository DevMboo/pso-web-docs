# llms.txt and machine-readable files

> The LLM-friendly artifacts generated with this portal, and how they are produced.

Every documentation build writes these files from the **same sources** as the HTML pages, so they cannot disagree with the portal.

| File | Content |
|---|---|
| [llms.txt](../../llms.txt) | index of every page as Markdown links with descriptions (pages that are not implemented are marked) |
| [llms-full.txt](../../llms-full.txt) | the whole documentation in one file |
| [ai/complete.md](../../ai/complete.md) | the complete AI context |
| [ai/security.md](../../ai/security.md) | security context |
| [ai/orchestrator.md](../../ai/orchestrator.md) | orchestrator context |
| [ai/microservices.md](../../ai/microservices.md) | Spring Boot context |
| [ai/dispatch.md](../../ai/dispatch.md) | dispatch context (rules for the implemented, optional dispatch subsystem) |
| [ai/cli.md](../../ai/cli.md) | CLI command reference context |
| [ai/AGENTS.md](../../ai/AGENTS.md), [ai/CLAUDE.md](../../ai/CLAUDE.md), [ai/copilot-instructions.md](../../ai/copilot-instructions.md) | the complete context in each assistant layout |
| [ai/manifest.json](../../ai/manifest.json) | version, scopes and SHA-256 of each AI file |
| `md/<page-id>.md` | every page as plain Markdown, with facts and examples expanded |

All of them carry the framework version (0.1.0). They contain no credentials, keys or environment-specific values: the build never reads `.env` files or key material.

Generate your own copy of the site:

```bash
pso docs --build ./site
```

Each file can be opened, copied or saved from the browser; the context builder also offers copy and download.
