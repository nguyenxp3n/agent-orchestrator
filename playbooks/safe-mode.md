# Playbook: Safe Mode Protocol

## Trigger
Canonical coordination state becomes corrupted, untrusted, or inconsistent (e.g., overlapping write paths, conflicting registries, or lost commit tracking).

## Procedure
1. **Freeze all active mutations**: Order all active coding workers to pause execution.
2. **Snapshot current state**: Record active Git branches, commit SHAs, worktrees, and uncommitted diffs.
3. **Reconstruct ground truth**: Inspect the repository on disk and recent commit history to verify actual filesystem state.
4. **Rebuild registries**: Reconstruct the Ownership Matrix and Resource Registry based on verified repository truth.
5. **Re-audit in-flight packages**: Validate candidate commits against the corrected baselines.
6. **Resume operations**: Increment assignment generation counters and resume dispatching.