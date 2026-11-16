# Context builder

> Choose and combine contexts, see the size, copy or download in the format your assistant reads.

Select the contexts your task needs. The **core rules** are always included, and scopes pull in the contexts they depend on. The text is assembled in your browser from the documentation shipped with this version; nothing is sent anywhere.



The size shown is an **estimate** (characters divided by four). It is not a tokenizer count and varies by model.

The same text is produced by the CLI, so you can generate it in a script:

```bash
pso ai:context --list
pso ai:context --scope security,module-routes --format markdown
pso ai:context --scope add-protected-route --format agents --output ./AGENTS.md --dry-run
```
