# Template: Resource Allocation Registry

## Allocation Table

| Resource ID | Type | Value/Slot | Owner WP | Mode | Status | Evidence/Notes |
|---|---|---|---|---|---|---|
| MIG- | migration | | | ALLOCATED_WRITE | reserved | |
| PORT- | port | | | ALLOCATED_WRITE | reserved | |
| DB- | table/schema | | | EXCLUSIVE_WRITE | active | |
| ROUTE- | route namespace | | | EXCLUSIVE/contract | active | |
| EVENT- | event/schema | | | contract | frozen | |

## Rules

- Do not store secret values; store only identifier/policy.
- Consumed migration/schema identifiers must not be recycled arbitrarily.
- Agent reassignment does not automatically change the WP owner.
- A new resource requires a `RESOURCE_REQUEST`/DR when collision risk exists.

## Allocation Check

```text
Duplicate active exclusive lease: NONE / list
Unallocated resource requested: NONE / list
Stale owner generation: NONE / list
```