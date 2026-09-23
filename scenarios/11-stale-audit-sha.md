# Scenario: Stale Audit SHA

## Trigger
Candidate có commit mới sau audit.

## Risk
Acceptance attestation không còn chứng minh current code.

## Evidence
Audit candidate SHA; current HEAD.

## Immediate Action
Invalidate direct acceptance for new SHA; inspect delta/re-audit.

## Forbidden Response
Không coi “chỉ docs” là tự động safe nếu docs là acceptance output.

## Recovery Procedure
Re-audit appropriate scope and issue fresh disposition.

## Exit Criteria
Accepted identity equals current candidate.

## Example Lead Response
```text
Audited abc123, worker pushes def456: report STALE_AUDIT_SHA.
```
