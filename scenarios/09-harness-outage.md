# Scenario: External Harness or Tool Outage

## Trigger
The LLM API, Git hosting platform, or CI runner environment experiences downtime.

## Risk
Corrupted coordination state or wasted retry budgets.

## Evidence
HTTP 5xx error responses, network timeouts, or unreachable remote repositories.

## Immediate Action
Preserve local state. Mark affected verification checks as `BLOCKED_ENVIRONMENT`.

## Forbidden Response
Never fail a work package implementation due to external infrastructure downtime.

## Recovery Procedure
Suspend active workers. Retain local worktrees and journals. Resume operations once external services recover.

## Exit Criteria
All affected tasks resume from verified local checkpoints.

## Example Lead Response
```text
CI runner unavailable. Marking CI check BLOCKED_ENVIRONMENT. Local state preserved. Pausing dispatch until service recovery.
```