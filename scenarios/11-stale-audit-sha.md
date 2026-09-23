# Scenario: Stale Audit SHA

## Trigger
The candidate receives a new commit after audit.

## Risk
The acceptance attestation no longer proves the current code.

## Evidence
Audit candidate SHA; current HEAD.

## Immediate Action
Invalidate direct acceptance for new SHA; inspect delta/re-audit.

## Forbidden Response
Do not treat “docs only” as automatically safe when documentation is an acceptance output.

## Recovery Procedure
Re-audit appropriate scope and issue fresh disposition.

## Exit Criteria
Accepted identity equals current candidate.

## Example Lead Response
```text
Audited abc123, worker pushes def456: report STALE_AUDIT_SHA.
```
