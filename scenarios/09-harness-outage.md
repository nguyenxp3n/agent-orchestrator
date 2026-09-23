# Scenario: Harness / Tool Outage

## Trigger
Agent provider, CI, Git host hoặc external tool unavailable.

## Risk
Misclassify environment failure as code failure; endless retries.

## Evidence
Provider/CI status; local state; last checkpoint.

## Immediate Action
Preserve state, mark environment block, pause affected checks.

## Forbidden Response
Không consume implementation retry budget vô hạn.

## Recovery Procedure
Resume from checkpoint when tool returns; rerun stale evidence.

## Exit Criteria
No state loss; final status distinguishes UNKNOWN/BLOCKED_ENVIRONMENT from FAIL_IMPLEMENTATION.

## Example Lead Response
```text
GitHub unavailable: local accepted evidence preserved, release CI remains UNKNOWN.
```
