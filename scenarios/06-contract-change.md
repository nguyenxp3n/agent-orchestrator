# Scenario: Contract Modification During Downstream Execution

## Trigger
An upstream worker alters a frozen API specification or database schema while downstream workers are implementing consumers.

## Risk
Downstream workers develop against obsolete contracts, guaranteeing integration failures.

## Evidence
Git diff on frozen contract path in the upstream candidate branch.

## Immediate Action
Halt all downstream workers consuming the altered contract.

## Forbidden Response
Never allow downstream workers to complete work against deprecated interfaces.

## Recovery Procedure
Assess contract compatibility. If breaking, publish an updated contract version, increment downstream generations, update worker contexts, and resume.

## Exit Criteria
All downstream workers complete against the updated, verified contract.

## Example Lead Response
```text
Halting WP-220. WP-100 modified review contract schema. Recompiling WP-220 context with updated types.
```