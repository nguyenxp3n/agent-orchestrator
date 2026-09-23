# Scenario: Corrupt Coordination State

## Trigger
Ownership/resource/WP records mâu thuẫn đến mức không biết canonical truth.

## Risk
Mỗi action mới có thể làm blast radius lớn hơn.

## Evidence
Git/workspaces; resource records; audit reports; active assignments.

## Immediate Action
Enter Safe Mode, freeze mutations, reconstruct truth.

## Forbidden Response
Không tiếp tục dispatch vì “có thể tự hồi phục”.

## Recovery Procedure
Rebuild ledger, revoke stale generations, re-audit affected candidates.

## Exit Criteria
Canonical state consistent and independently checkable.

## Example Lead Response
```text
Hai active owners cùng claim resource slot R3: freeze, reconstruct allocation history, resume only after one authoritative ownership truth.
```
