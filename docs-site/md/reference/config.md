# Configuration reference

> Typed configuration keys, defaults and constraints.

Layers, lowest to highest precedence: defaults, workspace, service, environment-file, environment-variables, cli. Environments: development, test, staging, production. See [Configuration resolution](concepts/configuration.md).

### server.host

Type `string`, default `"127.0.0.1"`.

### server.port

Type `number`, min 1024, max 65535.

### server.maxBodyBytes

Type `number`, default `1048576`, min 1.

### server.upstreamTimeoutMs

Type `number`, default `5000`, min 1.

### http.limits.maxBodyBytes

Type `number`, min 1, max 1073741824.

### http.limits.maxUpstreamResponseBytes

Type `number`, min 1, max 1073741824.

### http.limits.maxHeaderBytes

Type `number`, min 1024, max 1048576.

### http.limits.upstreamTimeoutMs

Type `number`, min 1, max 600000.

### http.limits.upstreamResponseTimeoutMs

Type `number`, min 1, max 3600000.

### http.limits.headersTimeoutMs

Type `number`, min 1, max 600000.

### http.limits.requestTimeoutMs

Type `number`, min 1, max 3600000.

### http.limits.keepAliveTimeoutMs

Type `number`, min 1, max 600000.

### http.limits.maxConnections

Type `number`, min 1, max 1000000.

### http.limits.maxConcurrentRequests

Type `number`, min 0, max 1000000.

### http.limits.maxUploadBytes

Type `number`, min 1, max 5368709120.

### http.limits.maxMultipartParts

Type `number`, min 1, max 1000.

### http.limits.maxConcurrentUploads

Type `number`, min 0, max 100000.

### http.readiness.optional

Type `list`.

### http.readiness.intervalMs

Type `number`, min 250, max 300000.

### http.readiness.timeoutMs

Type `number`, min 50, max 60000.

### http.shutdown.graceMs

Type `number`, min 0, max 300000.

### http.shutdown.cleanupMs

Type `number`, min 1000, max 120000.

### http.rateLimit.perMinute

Type `number`, min 0.

### http.rateLimit.burst

Type `number`, min 1.

### http.rateLimit.provider

Type `string`, one of local, redis.

### http.rateLimit.redisUrl

Type `string`.

### http.rateLimit.onFailure

Type `string`, one of open, closed.

### http.rateLimit.perPrincipal

Type `boolean`.

### http.rateLimit.maxKeys

Type `number`, min 100, max 5000000.

### http.rateLimit.prefix

Type `string`.

### http.rateLimit.trustedProxies

Type `list`.

### http.rateLimit.routes

Type `json`.

### http.diagnostics.token

Type `string`.

### security.authentication.provider

Type `string`, default `"jwt"`, one of jwt, custom.

### security.authentication.customProvider

Type `string`.

### security.authentication.issuer

Type `string`.

### security.authentication.audience

Type `list`.

### security.authentication.algorithms

Type `list`, default `["ES256","RS256"]`.

### security.authentication.clockToleranceSeconds

Type `number`, default `5`, min 0, max 60.

### security.authentication.maxTokenBytes

Type `number`, default `8192`, min 256, max 16384.

### security.authentication.tokenType

Type `string`.

### security.authentication.rolesClaim

Type `string`, default `"roles"`.

### security.authentication.permissionsClaim

Type `string`, default `"permissions"`.

### security.authentication.jwks.uri

Type `string`.

### security.authentication.jwks.timeoutMs

Type `number`, default `3000`, min 200, max 15000.

### security.authentication.jwks.cacheTtlSeconds

Type `number`, default `600`, min 30, max 86400.

### security.authentication.jwks.trustedHosts

Type `list`.

### security.authentication.jwks.connectTimeoutMs

Type `number`, default `2000`, min 100, max 15000.

### security.authentication.jwks.initDeadlineMs

Type `number`, default `20000`, min 0, max 300000.

### security.authentication.jwks.privateHosts

Type `list`.

### security.authentication.jwks.caFile

Type `string`.

### security.authentication.publicKey.file

Type `string`.

### security.authorization.defaultPolicy

Type `string`, default `"authenticated"`, one of authenticated.

### security.propagation.forward

Type `string`, default `"bearer"`, one of bearer, none.

### security.jwt.issuer

Type `string`.

### security.jwt.audience

Type `string`.

### security.jwt.jwksUri

Type `string`.

### security.jwt.publicKeyFile

Type `string`.

### security.jwt.algorithms

Type `string[]`, default `["RS256","ES256"]`.

### security.rateLimit.perMinute

Type `number`, default `600`, min 1.

### logging.level

Type `string`, default `"info"`, one of debug, info, warn, error.

### logging.format

Type `string`, one of pretty, json.

### logging.color

Type `string`, one of auto, always, never, detect.

### logging.errorDetails

Type `boolean`, default `false`.

### dispatch.enabled

Type `boolean`, default `false`.

### dispatch.provider

Type `string`, default `"none"`, one of none, redis, rabbitmq, kafka.

### dispatch.maxMessageBytes

Type `number`, default `1048576`, min 1024, max 16777216.

### dispatch.consumer.concurrency

Type `number`, default `1`, min 1, max 64.

### dispatch.consumer.batchSize

Type `number`, default `10`, min 1, max 1000.

### dispatch.retry.enabled

Type `boolean`, default `true`.

### dispatch.retry.maxAttempts

Type `number`, default `3`, min 1, max 20.

### dispatch.retry.initialDelay

Type `string`, default `"1s"`.

### dispatch.retry.maxDelay

Type `string`, default `"30s"`.

### dispatch.retry.multiplier

Type `number`, default `2`, min 1, max 10.

### dispatch.deadLetter.enabled

Type `boolean`, default `true`.

### dispatch.observability.enabled

Type `boolean`, default `true`.

### dispatch.idempotency.retention

Type `string`, default `"24h"`.

### dispatch.idempotency.lease

Type `string`, default `"30s"`.

### dispatch.idempotency.store

Type `string`, default `"auto"`, one of auto, memory, redis.

### dispatch.idempotency.redisUrl

Type `string`.

### dispatch.trust.mode

Type `string`, default `"off"`, one of off, permissive, required.

### dispatch.redis.url

Type `string`.

### dispatch.redis.maxLength

Type `number`, default `100000`, min 100.

### dispatch.redis.minIdle

Type `string`, default `"60s"`.

### dispatch.redis.block

Type `string`, default `"2s"`.

### dispatch.redis.streamCreation

Type `string`, default `"auto"`, one of auto, require.

### dispatch.rabbitmq.url

Type `string`.

### dispatch.rabbitmq.exchangePrefix

Type `string`, default `"pso."`.

### dispatch.kafka.brokers

Type `list`.

### dispatch.kafka.clientId

Type `string`.

### dispatch.kafka.topicCreation

Type `string`, default `"auto"`, one of auto, require.

### dispatch.kafka.partitions

Type `number`, default `1`, min 1, max 1000.

### dispatch.kafka.replicationFactor

Type `number`, default `1`, min 1, max 10.

### dispatch.kafka.ssl

Type `boolean`, default `false`.

### dispatch.kafka.sasl.mechanism

Type `string`, one of plain, scram-sha-256, scram-sha-512.

### dispatch.kafka.sasl.username

Type `string`.

### dispatch.kafka.sasl.password

Type `string`.
