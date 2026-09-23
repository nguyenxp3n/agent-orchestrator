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

A verifiable outcome, not a vague description such as “improve module.”

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

List by layer: database, backend, frontend/mobile, contracts, tests, docs, generated artifacts.

## Acceptance Criteria

Every criterion must be measurable and have an expected evidence source/command.

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

The Worker submits changed files, HEAD SHA, expected-output check, commands + exit codes, resource usage, unresolved items, and requests. The Worker does not self-accept.