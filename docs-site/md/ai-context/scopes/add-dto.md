# Task: add a DTO

> Exact steps to add a DTO and validate its input.

- Orchestrator DTO: `pso make:dto <module> <name> --service <orchestrator>` creates `src/routes/<module>/dto/<name>.dto.ts` (`@Dto() export class <Name>Dto {}`; add fields with `--fields "name:string:min=3,email:email,age?:integer:min=18"`). Add or edit fields, then use it in an action: `create(@Body() dto: CreateUserDto)`.
- **`@Body(MyDto)` validates; `@Body()` does not.** The class must be decorated `@Dto()` and each field needs a type decorator (`@StringField()`, `@IntegerField()`, `@Email()`, ...) plus `@Required()`/`@Optional()`; TypeScript types and `?` are erased and are never read. Unknown properties are rejected by default. Invalid input is a `400` `PSO_VALIDATION_FAILED` problem and the action does not run. Use `pso make:route <module> <action> --method POST --path / --dto <name>` to generate `@Body(<Name>Dto)`. Rules that need a service are `@Validate(Class)` providers listed in `MODULE.pso.ts`. When the action only forwards to a Spring service, keep `@Body()` and let the service validate.
- Spring DTO: `pso make:dto <name> --service <spring-service>` creates `<Name>Dto` (a record with `@NotBlank` as a starting point) in the `contracts` package. Use `@Valid @RequestBody` in controllers; invalid input answers `400`.
- Do not share a DTO class between services. If both sides need the shape, keep a service contract (`*.contract.json`) and mirror the structure in each language.
- Run `pso validate` afterwards.
