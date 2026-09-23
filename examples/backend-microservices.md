# Example: Backend Microservices

## Intake

Discover service boundaries, proto/OpenAPI/event schemas, database ownership, message broker topics, the local integration environment, and CI commands.

## Work Packages

```text
WP-CONTRACT: freeze proto/events
WP-USER: user service changes
WP-BILLING: billing service changes
WP-NOTIFY: notification consumer
WP-MIGRATIONS: schema slots per database owner
WP-INTEGRATION: contract/E2E
```

## Semantic Ownership

Paths may be independent while event names and protobuf messages remain shared contracts. Mark schema files read-only/frozen for workers; contract changes must go through DR/WP-CONTRACT.

## Verification

```bash
go test ./...
# or project equivalent
<contract-compatibility-check>
<service-integration-test>
```

Do not require Docker when the project uses testcontainers, local services, or a remote ephemeral environment. The evidence level must state which environment was verified.