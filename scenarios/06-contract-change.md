# Scenario: Contract Change During Downstream Work

## Trigger
Frozen API/schema/event contract thay khi consumers đang implement.

## Risk
Workers hoàn tất trên assumptions cũ.

## Evidence
Contract version/SHA; downstream context versions.

## Immediate Action
Pause affected WPs; classify breaking/non-breaking; refresh context.

## Forbidden Response
Không để workers tiếp tục rồi sửa conflict ở cuối.

## Recovery Procedure
Issue new contract version/generation and reverify affected work.

## Exit Criteria
Downstream candidates prove compatibility with current contract.

## Example Lead Response
```text
OpenAPI payload field changed: frontend/backend consumers phải refresh trước completion.
```
