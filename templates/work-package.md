# Template: Work Package Contract

## Identity

```text
WP_ID:
Title:
Owner role:
Priority:
Assignment generation:
Base SHA:
Branch/workspace:
```

## Objective

Một outcome có thể kiểm chứng, không mô tả mơ hồ kiểu “improve module”.

## Source Requirements

- Requirement/spec references:
- Frozen contract references:
- Upstream accepted artifacts:

## Dependencies

```text
Hard:
Contract/frozen:
Soft:
Integration:
```

## Permission Envelope

```yaml
allowed_paths: []
readonly_paths: []
forbidden_paths: []
```

## Resource Allocations

```text
Migration Slot:
Ports:
DB objects:
Routes/events/queues:
Env vars/feature flags:
Other exclusive resources:
```

## Expected Outputs

Liệt kê theo tầng: database, backend, frontend/mobile, contracts, tests, docs, generated artifacts.

## Acceptance Criteria

Mỗi criterion phải measurable và có evidence source/command dự kiến.

## Verification Commands

```text
Target tests:
Repo/global gate:
Contract/schema checks:
Special security/regression checks:
```

## Stop Conditions

Scope/resource expansion; spec conflict; security decision; destructive action; stale upstream contract; workspace/base mismatch.

## Completion Contract

Worker gửi changed files, HEAD SHA, expected-output check, commands + exit codes, resource usage, unresolved items và requests. Worker không self-accept.