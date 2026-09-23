# Scenario: Integration Hotspot Conflict

## Trigger
Accepted branches conflict tại router/bootstrap/root config.

## Risk
Integrator có thể vô tình chọn semantics sai.

## Evidence
Integration Requests; frozen contracts; diff from each candidate.

## Immediate Action
Resolve only when semantics authoritative; otherwise DR.

## Forbidden Response
Không chọn ours/theirs chỉ để compile.

## Recovery Procedure
Apply approved hotspot changes, run global gate.

## Exit Criteria
Integrated result matches contracts and tests pass.

## Example Lead Response
```text
Hai modules cùng cần route registration; Integrator kết hợp registrations theo contract.
```
