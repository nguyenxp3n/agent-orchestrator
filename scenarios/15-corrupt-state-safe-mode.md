# Scenario: Corrupt Coordination State

## Trigger
Ownership/resource/WP records conflict so severely that canonical truth cannot be established.

## Risk
Every new action can increase blast radius.

## Evidence
Git/workspaces; resource records; audit reports; active assignments.

## Immediate Action
Enter Safe Mode, freeze mutations, reconstruct truth.

## Forbidden Response
Do not continue dispatch because “it may recover on its own.”

## Recovery Procedure
Rebuild ledger, revoke stale generations, re-audit affected candidates.

## Exit Criteria
Canonical state consistent and independently checkable.

## Example Lead Response
```text
Two active owners claim resource slot R3: freeze, reconstruct allocation history, resume only after one authoritative ownership truth remains.
```
