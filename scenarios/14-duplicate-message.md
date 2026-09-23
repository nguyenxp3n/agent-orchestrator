# Scenario: Duplicate / Stale Agent Message

## Trigger
Agent/harness sends a completion/decision message again.

## Risk
Double transition, duplicate merge/resource allocation.

## Evidence
Message/task/generation identity; current canonical state.

## Immediate Action
Ignore duplicate idempotently; reject stale generation.

## Forbidden Response
Do not advance state twice because the same report arrives twice.

## Recovery Procedure
If payload differs under same identity, flag integrity issue/Safe Mode as needed.

## Exit Criteria
One canonical transition per logical message.

## Example Lead Response
```text
A replayed Completion Report does not create a second ACCEPTED/integration action.
```
