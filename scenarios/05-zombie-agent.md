# Scenario: Zombie Agent Returns

## Trigger
Worker cũ quay lại sau khi WP đã reassigned.

## Risk
Stale commits/messages có thể ghi đè work mới.

## Evidence
Assignment generation; message generation; current owner.

## Immediate Action
Reject stale generation output.

## Forbidden Response
Không merge vì “agent cũ đã hoàn thành nhiều hơn”.

## Recovery Procedure
Nếu artifact hữu ích, treat như external candidate và reconcile qua explicit new task, không overwrite state.

## Exit Criteria
Canonical WP state chỉ nhận current generation.

## Example Lead Response
```text
Generation 2 return sau generation 3: report STALE_GENERATION.
```
