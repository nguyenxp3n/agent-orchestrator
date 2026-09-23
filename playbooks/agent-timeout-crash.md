# Playbook: Agent Timeout or Crash Recovery

## Trigger
A coding worker terminates unexpectedly, times out, or stops responding.

## Procedure
1. **Preserve workspace**: Do not delete the worktree. Capture the active state:
   ```bash
   git status --short
   git diff
   git log -3 --oneline
   ```
2. **Catalog state**: Record uncommitted changes, modified paths, and active resource locks.
3. **Classify status**:
   - Clean: Work was committed cleanly up to a known commit SHA.
   - Dirty: Uncommitted modifications exist in the working tree.
   - Corrupted: Incomplete file writes or invalid syntax across files.
4. **Increment generation**: Increment the `ASSIGNMENT_GENERATION` counter for this Work Package.
5. **Dispatch recovery**: Dispatch a fresh worker with the Recovery Bundle containing captured diffs and the incremented generation counter. If the previous worker subsequently resumes, its outputs are rejected as stale.