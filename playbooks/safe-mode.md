# Playbook: Safe Mode

## Trigger
Canonical coordination state is untrustworthy: duplicate ownership, resource registry conflict, unknown active writer, corrupt state, or conflicting evidence identity.

## Procedure
1. Freeze dispatch/integration mutations.
2. Snapshot Git/workspaces/resource records.
3. Reconstruct truth from repository + evidence.
4. Resolve contradictions and revoke stale generations.
5. Rebuild ownership/resource ledger.
6. Re-audit affected candidates.
7. Resume from the new baseline.

```text
SAFE MODE means stop making the uncertain state worse.
```

## Exit Criteria
One consistent canonical state is proven; no active collision remains; affected WPs have a new generation/baseline.
