# Scenario: Zombie Agent Returns After Reassignment

## Trigger
A crashed or timed-out worker resumes execution and submits a completion report after a replacement worker has been dispatched.

## Risk
Overwriting valid recovery work with stale or corrupted state.

## Evidence
Incoming report generation counter is lower than active ledger generation.

## Immediate Action
Reject the submission immediately based on generation fencing.

## Forbidden Response
Never accept outputs from an expired generation simply because tests appear to pass.

## Recovery Procedure
Terminate the zombie worker process. Confirm active worker holds the current generation counter.

## Exit Criteria
Only outputs from the active generation counter enter the audit queue.

## Example Lead Response
```text
Rejected submission from Worker-2 (Generation 1). Active generation is 2. Zombie output discarded.
```