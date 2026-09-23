# Scenario: Resource Identifier Collision

## Trigger
Two workers generate identical database migration sequence numbers, claim the same network port, or bind the same route namespace.

## Risk
Migration failure upon deployment, runtime port binding collisions, or overlapping endpoint routes.

## Evidence
Resource Registry showing duplicate sequence slots or overlapping namespaces.

## Immediate Action
Block integration for both branches until unique sequence slots are assigned.

## Forbidden Response
Never permit workers to guess the next number by inspecting local folder listings.

## Recovery Procedure
Assign explicit unique resource slots in the Resource Registry. The affected worker renames artifacts, updates tests, and commits the fix.

## Exit Criteria
Every branch claims distinct allocated identifiers; migration tests pass upgrade and downgrade cycles.

## Example Lead Response
```text
Collision on migration 000021. WP-A holds Slot R1 (000021). Reassigning WP-B to Slot R2 (000022). Update filenames and re-audit.
```