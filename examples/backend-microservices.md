# Example: Backend Microservices

## Intake

Discover service boundaries, proto/OpenAPI/event schemas, database ownership, message broker topics, local integration environment và CI commands.

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

Paths có thể độc lập nhưng event names và protobuf messages là shared contracts. Đặt schema files read-only/frozen cho workers; contract change phải qua DR/WP-CONTRACT.

## Verification

```bash
go test ./...
# or project equivalent
<contract-compatibility-check>
<service-integration-test>
```

Không bắt Docker nếu project dùng testcontainers, local services hay remote ephemeral environment. Evidence level phải nói rõ environment nào đã được kiểm.