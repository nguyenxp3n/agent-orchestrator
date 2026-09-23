# Playbook: Agent Timeout / Crash

## Trigger
Worker loses heartbeat, times out, process crashes, or provider session disappears.

## Freeze and Capture
Do not delete the workspace. Capture `git status`, `git diff`, recent commits, leases, DRs, and warnings.

```bash
git status --short
git diff
git log -3 --oneline
```

## Recovery
Classify as clean recoverable / dirty recoverable / corrupted / unsafe-unknown. Create a Recovery Bundle, increment assignment generation, and transfer WP resources to the new worker. Reject output from the old generation.

## Exit Criteria
The new Worker receives the exact safe commit/context/resources; the zombie generation is fenced; remaining acceptance criteria are explicit.
