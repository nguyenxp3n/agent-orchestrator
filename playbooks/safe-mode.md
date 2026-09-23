# Playbook: Safe Mode

## Trigger
Canonical coordination state không đáng tin: duplicate ownership, resource registry conflict, unknown active writer, corrupt state hoặc evidence identity mâu thuẫn.

## Procedure
1. Freeze dispatch/integration mutations.
2. Snapshot Git/workspaces/resource records.
3. Reconstruct truth từ repository + evidence.
4. Resolve contradictions và revoke stale generations.
5. Rebuild ownership/resource ledger.
6. Re-audit affected candidates.
7. Resume với baseline mới.

```text
SAFE MODE means stop making the uncertain state worse.
```

## Exit Criteria
Một canonical state nhất quán được chứng minh; không còn active collision; affected WPs có generation/baseline mới.
