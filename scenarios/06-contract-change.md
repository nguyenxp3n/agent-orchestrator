# Scenario: Contract Change During Downstream Work

## Trigger
A frozen API/schema/event contract changes while consumers are implementing.

## Risk
Workers complete work against stale assumptions.

## Evidence
Contract version/SHA; downstream context versions.

## Immediate Action
Pause affected WPs; classify breaking/non-breaking; refresh context.

## Forbidden Response
Do not let workers continue and postpone conflict resolution until the end.

## Recovery Procedure
Issue new contract version/generation and reverify affected work.

## Exit Criteria
Downstream candidates prove compatibility with current contract.

## Example Lead Response
```text
OpenAPI payload field changed: frontend/backend consumers must refresh before completion.
```
