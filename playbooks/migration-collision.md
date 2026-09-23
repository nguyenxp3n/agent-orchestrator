# Playbook: Migration Collision

## Trigger
Two branches use the same migration ID, or a worker needs an unallocated migration.

## Immediate Action
Stop affected integration. Determine the slot owner from the Resource Registry; do not renumber based on intuition.

```bash
find migrations -maxdepth 1 -type f | sort
git diff <base>...HEAD -- migrations/
```

## Resolution
Preserve the valid owner's slot; allocate a new slot to the other WP; update filenames/references/tests; verify dependency ordering; if candidate identity changes, re-audit.

## Exit Criteria
No duplicate ID, valid dependency sequence, passing up/down semantics, and a new audit bound to the correct SHA.
