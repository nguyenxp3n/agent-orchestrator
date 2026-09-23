# Scenario: Duplicate / Stale Agent Message

## Trigger
Agent/harness gửi lại completion/decision message.

## Risk
Double transition, duplicate merge/resource allocation.

## Evidence
Message/task/generation identity; current canonical state.

## Immediate Action
Ignore duplicate idempotently; reject stale generation.

## Forbidden Response
Không tăng state hai lần vì cùng một report tới hai lần.

## Recovery Procedure
If payload differs under same identity, flag integrity issue/Safe Mode as needed.

## Exit Criteria
One canonical transition per logical message.

## Example Lead Response
```text
Completion report replay không tạo second ACCEPTED/integration action.
```
