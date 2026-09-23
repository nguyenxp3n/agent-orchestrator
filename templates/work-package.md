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

A concrete, measurable outcome. Avoid vague descriptions like "improve service."

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
Migration slot:
Ports:
Database objects:
Routes/events/queues:
Environment variables / feature flags:
Other exclusive resources:
```

## Expected Outputs

List deliverables by layer: database migrations, backend services, frontend/mobile views, contracts, tests, documentation, and generated artifacts.

## Acceptance Criteria

Every criterion must be measurable with an expected evidence source or test command.

## Verification Commands

```text
Targeted tests:
Repository global gate:
Contract/schema validation:
Security/regression checks:
```

## Stop Conditions

Scope or resource expansion required; specification conflict; security decision; destructive database modification; stale upstream contract; workspace baseline mismatch.

## Completion Contract

Worker submits changed files, HEAD commit SHA, deliverable checklist, test commands and exit codes, resource usage, unresolved items, and formal requests. Workers never self-accept.