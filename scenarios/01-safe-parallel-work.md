# Scenario: Safe Parallel Work

## Trigger
Two independent Work Packages are ready with zero path or resource overlaps.

## Risk
Wasting concurrency through unnecessary serialization, or causing collisions through false assumptions of independence.

## Evidence
Ownership Matrix, Resource Registry, dependency DAG, and frozen interface contracts.

## Immediate Action
Schedule packages in the same parallel execution wave following readiness checks.

## Forbidden Response
Never grant broad write permissions simply because two tasks appear distinct.

## Recovery Procedure
If an unexpected overlap surfaces during execution, halt the affected package and realign ownership boundaries.

## Exit Criteria
Both candidate branches pass independent audits with zero cross-package regressions.

## Example Lead Response
```text
WP-A and WP-B share zero exclusive paths or resources. Dispatching concurrently in isolated worktrees.
```