# Scenario: Harness / Tool Outage

## Trigger
Agent provider, CI, Git host, or an external tool is unavailable.

## Risk
Misclassify environment failure as code failure; endless retries.

## Evidence
Provider/CI status; local state; last checkpoint.

## Immediate Action
Preserve state, mark environment block, pause affected checks.

## Forbidden Response
Do not consume implementation retry budget indefinitely.

## Recovery Procedure
Resume from checkpoint when tool returns; rerun stale evidence.

## Exit Criteria
No state loss; final status distinguishes UNKNOWN/BLOCKED_ENVIRONMENT from FAIL_IMPLEMENTATION.

## Example Lead Response
```text
GitHub unavailable: local accepted evidence preserved, release CI remains UNKNOWN.
```
