# Quick Reference: Decision Tree

## Có thể dispatch WP?

```text
Hard deps satisfied? no -> WAIT
Contract frozen if required? no -> WAIT/DR
Path collision? yes -> REPLAN
Resource collision? yes -> REALLOCATE
Workspace isolated? no -> PROVISION/FALLBACK
Critical unknown? yes -> DR
else -> DISPATCH
```

## Worker cần sửa ngoài scope?

```text
Can solve in scope? yes -> clarify
Shared hotspot? yes -> Integration Request
Owned by another WP? yes -> reject/transfer/new WP
Security/public contract/destructive? yes -> DR/human authority
Else bounded extension -> record + extra verification
```

## Worker reports DONE?

```text
Completion Report -> exact SHA -> Forensic Audit
missing output/violation/test fail -> REWORK
all required evidence pass -> ACCEPTED
```

## Merge conflict?

```text
Text-only + same semantics -> Integrator resolves
Frozen contract decides -> follow contract
Semantics ambiguous -> DR
Ownership violation -> re-plan/corrective work
```