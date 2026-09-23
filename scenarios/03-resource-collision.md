# Scenario: Resource / Migration Collision

## Trigger
Two WPs request the same migration number, port, route, or DB object.

## Risk
Runtime conflict, migration sequence break, semantic corruption.

## Evidence
Resource Allocation Registry; current migrations; active leases.

## Immediate Action
Block the second allocation; preserve the valid owner and allocate a different slot.

## Forbidden Response
Do not independently choose the “next number” on a private branch.

## Recovery Procedure
Update the resource registry, candidate references, and re-audit the changed candidate.

## Exit Criteria
No duplicate exclusive resource remains and ordering is valid.

## Example Lead Response
```text
Resource slot R2 already belongs to WP-B; WP-INFRA must not self-claim R2 or create another shared identifier. If the project uses a DB, R2 may be a migration slot; otherwise use the actual resource type.
```
