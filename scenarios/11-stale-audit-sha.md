# Scenario: Candidate Commit Differs From Audited Commit

## Trigger
A worker pushes an additional commit to its branch after an audit has been completed.

## Risk
Merging uninspected, unverified code into main.

## Evidence
Branch HEAD commit SHA differs from the candidate commit SHA recorded in the audit report.

## Immediate Action
Block merge. Transition status from `ACCEPTED` to `STALE_AUDIT_SHA`.

## Forbidden Response
Never assume a minor post-audit commit is safe without inspection.

## Recovery Procedure
Review the diff between the audited commit and branch HEAD. Re-execute test commands and issue a fresh audit report.

## Exit Criteria
The exact commit SHA present at branch HEAD holds a verified, passing audit report.

## Example Lead Response
```text
Candidate SHA mismatch. Audited commit was a1b2c3d; branch HEAD is e4f5g6h. Re-executing forensic audit against e4f5g6h.
```