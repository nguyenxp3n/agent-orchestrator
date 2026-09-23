# Scenario: Integration Hotspot Conflict

## Trigger
Accepted branches conflict at router/bootstrap/root config.

## Risk
The Integrator may accidentally choose incorrect semantics.

## Evidence
Integration Requests; frozen contracts; diff from each candidate.

## Immediate Action
Resolve only when semantics authoritative; otherwise DR.

## Forbidden Response
Do not choose ours/theirs merely to make the code compile.

## Recovery Procedure
Apply approved hotspot changes, run global gate.

## Exit Criteria
Integrated result matches contracts and tests pass.

## Example Lead Response
```text
Two modules require route registration; Integrator combines the registrations according to the contract.
```
