# Scenario: Path Ownership Collision

## Trigger
Two active workers attempt to edit the same file path concurrently.

## Risk
Merge conflicts, overwritten work, or race conditions.

## Evidence
Ownership Matrix showing overlapping write boundaries in active worktrees.

## Immediate Action
Halt execution on the colliding package. Reassign path ownership or serialize execution.

## Forbidden Response
Never advise workers to "resolve it manually later during merge."

## Recovery Procedure
Reconstruct boundaries. Reassign the disputed path to one worker, mark it read-only for the other, and recompile task prompts.

## Exit Criteria
Disputed path is owned exclusively by one worker; dependent worker receives the frozen contract.

## Example Lead Response
```text
Halt WP-B. Path `services/api/router.go` belongs to WP-A. WP-B will receive this dependency via Integration Request.
```