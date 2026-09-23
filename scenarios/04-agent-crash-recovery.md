# Scenario: Agent Timeout or Crash Recovery

## Trigger
A coding worker process terminates, times out, or stops responding.

## Risk
Lost work, lingering locks, or corrupted working trees.

## Evidence
Process timeout logs, absent progress reports, uncommitted workspace diffs.

## Immediate Action
Freeze the worktree. Capture uncommitted diffs and last known commit SHA.

## Forbidden Response
Never wipe the workspace immediately without preserving working diffs.

## Recovery Procedure
Construct a Recovery Bundle containing captured state. Increment `ASSIGNMENT_GENERATION`. Reassign to a fresh worker.

## Exit Criteria
New worker completes the package; crashed worker's stale outputs are rejected.

## Example Lead Response
```text
Worker-2 timed out. Capturing diff in wt-wp210. Incrementing generation to 2. Dispatching fresh worker with Recovery Bundle.
```