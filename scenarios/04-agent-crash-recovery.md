# Scenario: Agent Crash / Recovery

## Trigger
Worker times out/crashes while the WP is RUNNING.

## Risk
Risk of lost work, duplicate work, or resource leakage.

## Evidence
Workspace status/diff; last safe commit; leases; pending DRs.

## Immediate Action
Freeze workspace, capture Recovery Bundle, classify recoverability.

## Forbidden Response
Do not delete the worktree or immediately return resources to the pool.

## Recovery Procedure
Reassign the WP with a new generation and preserved safe state.

## Exit Criteria
The new Worker continues from safe evidence; the old generation is invalid.

## Example Lead Response
```text
Agent-4 crashes at generation 2; Agent-7 receives generation 3 with the same allocated resource slot R2 for the WP.
```
