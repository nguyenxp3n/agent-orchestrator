# Scenario: Safe Parallel Work

## Trigger
Two independent WPs are ready with no path/resource overlap.

## Risk
Unnecessary serialization wastes concurrency; an incorrect independence assumption causes collision.

## Evidence
Ownership Matrix; resource registry; hard dependencies; frozen contracts.

## Immediate Action
Schedule both in the same parallel wave after the readiness gate.

## Forbidden Response
Do not grant broad permissions merely because two tasks “look different.”

## Recovery Procedure
If overlap is discovered mid-execution, pause the affected WP and re-plan ownership.

## Exit Criteria
Both candidates pass independent audit and create no cross-WP regression.

## Example Lead Response
```text
WP-A and WP-B share no exclusive path/resource; dispatch them in parallel in separate worktrees.
```
