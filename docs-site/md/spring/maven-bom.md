# Maven BOM and SNAPSHOTs

> How generated services get compatible PSO versions, how to build the artifacts locally and what releases must look like.

> Status: PARTIALLY implemented — read the limitations.

Services import one bill of materials and then declare starters without versions:

```xml
<properties>
  <pso.version>0.1.0-SNAPSHOT</pso.version>
</properties>
<dependencyManagement>
  <dependencies>
    <dependency>
      <groupId>com.pso</groupId>
      <artifactId>pso-bom</artifactId>
      <version>${pso.version}</version>
      <type>pom</type>
      <scope>import</scope>
    </dependency>
  </dependencies>
</dependencyManagement>
```

The `pso.version` property is the only place a PSO version appears. Spring Boot itself stays managed by `spring-boot-starter-parent` 3.5.6 (Java 21 target).

## Local development with SNAPSHOTs

```bash
pso java:install        # builds pso-java/ and installs com.pso:*:0.1.0-SNAPSHOT into ~/.m2
pso create service users-service
```

No JAR is copied by hand. `pso create service` tells you when the artifacts are missing from the local repository. Change the starters, run `pso java:install` again, and every service picks up the new SNAPSHOT.

The group id `com.pso` is **not** an owned Maven Central namespace: the artifacts are never published from this repository. Choose and register a namespace before publishing.

## Releases

Production builds must use an immutable version: set `pso.version` to a release, never a `-SNAPSHOT`. `pso security:audit` reports SNAPSHOT dependencies and `pso deploy` refuses to deploy a SNAPSHOT build.

## Containers

`docker:init` builds a Spring service inside a Docker build, where your local `~/.m2` is not visible. The `com.pso:*` artifacts must therefore be resolvable from a Maven repository the build can reach (publish them to your registry). Building such an image has **not** been verified: Docker is not available in the development environment.

## Common mistakes

- Overriding a Spring Boot-managed version next to the PSO BOM.
- Building in a container without a repository that has the `com.pso` artifacts ([Maven dependencies](microservices/maven-dependencies.md)).
- Deploying a `-SNAPSHOT` `pso.version` (refused by `pso deploy`).

## Next steps

- [Maven dependencies](microservices/maven-dependencies.md)
- [Build and deploy](devops/build-and-deploy.md)
