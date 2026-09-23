# Scenario: Forbidden Path Write

## Trigger
Worker modifies a readonly/forbidden/integration-only file.

## Risk
Boundary breach, hidden coupling, collision.

## Evidence
git diff name-status; WP permission envelope.

## Immediate Action
Reject audit and issue focused rework or approved transfer if truly required.

## Forbidden Response
Do not keep the change because “it makes tests pass.”

## Recovery Procedure
Revert/move work to authorized owner; rerun tests and audit.

## Exit Criteria
Candidate diff entirely authorized.

## Example Lead Response
```text
Worker edits .github/workflows despite backend-only WP: REJECT_REWORK.
```
