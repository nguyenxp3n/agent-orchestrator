# Playbook: Scope Expansion

## Trigger
Worker needs to modify a path/resource outside the WP to satisfy acceptance.

## Decision Procedure
1. Prove the change is mandatory, not merely convenient.
2. Check the Ownership Matrix and active workers.
3. Find an in-scope solution or Integration Request first.
4. If scope must expand, grant the exact path/resource, not a broad wildcard.
5. Record invariants and additional verification.

```text
GRANT: Taskfile.yml only
PRESERVE: all existing tasks
VERIFY: task qa + task spec:validate
NO unrelated refactor
```

## Reject When
Reject when the scope belongs to another WP, a shared hotspot is active, the worker wants to self-allocate a migration/port, or the change violates a frozen contract.

## Exit Criteria
A decision record exists; ownership/resource matrix is updated; affected workers/context are refreshed; audit is aware of the new scope.
