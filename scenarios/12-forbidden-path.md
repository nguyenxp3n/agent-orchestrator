# Scenario: Worker Writes to Forbidden Path

## Trigger
Audit diff reveals that a worker created, modified, or deleted files outside its assigned `allowed_paths`.

## Risk
Overwriting shared configurations or introducing uncoordinated side effects.

## Evidence
`git diff <base>...<candidate> --name-status` shows paths outside assigned envelope.

## Immediate Action
Issue an immediate `REJECT/REWORK` disposition with a `SCOPE_VIOLATION` finding.

## Forbidden Response
Never forgive boundary violations because the out-of-scope code looks useful.

## Recovery Procedure
Worker reverts out-of-scope modifications. If the modification was genuinely necessary, file an explicit Scope Expansion Request.

## Exit Criteria
Candidate diff strictly respects assigned path boundaries; all required tests pass.

## Example Lead Response
```text
Audit rejected: SCOPE_VIOLATION. Worker modified services/api/router.go. Revert changes to router.go and file an Integration Request.
```