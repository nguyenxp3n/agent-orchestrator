# Scenario: Agent Crash / Recovery

## Trigger
Worker timeout/crash khi WP đang RUNNING.

## Risk
Mất work, duplicate work, resource leak.

## Evidence
Workspace status/diff; last safe commit; leases; pending DRs.

## Immediate Action
Freeze workspace, capture Recovery Bundle, classify recoverability.

## Forbidden Response
Không xóa worktree hoặc trả resource về pool ngay.

## Recovery Procedure
Reassign WP với generation mới và preserved safe state.

## Exit Criteria
New worker tiếp tục từ safe evidence; old generation invalid.

## Example Lead Response
```text
Agent-4 crash ở generation 2; Agent-7 nhận generation 3 với cùng allocated resource slot R2 của WP.
```
