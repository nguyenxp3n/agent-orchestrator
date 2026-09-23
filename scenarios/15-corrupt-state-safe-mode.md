# Scenario: Coordination State Corruption (Safe Mode)

## Trigger
Registries become inconsistent, ownership boundaries overlap, or active commit tracking is lost.

## Risk
System-wide breakdown of coordination leading to widespread code collisions.

## Evidence
Contradictory entries in Ownership Matrix, missing commit references, or conflicting migration numbers.

## Immediate Action
Declare `SAFE_MODE`. Freeze all active worker dispatches and integration merges immediately.

## Forbidden Response
Never attempt to resolve corrupted state while workers continue active write operations.

## Recovery Procedure
Execute Safe Mode protocol: snapshot disk state, inspect repository commit history, rebuild registries from ground truth, re-audit candidates, and resume.

## Exit Criteria
Registries rebuilt and verified against disk; operations resume under updated baselines.

## Example Lead Response
```text
SAFE MODE ACTIVATED. Halting all worker dispatches. Freezing active branches. Rebuilding coordination state from repository truth.
```