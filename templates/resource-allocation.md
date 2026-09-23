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

- Không lưu secret value; chỉ lưu identifier/policy.
- Consumed migration/schema identifiers không recycle tùy tiện.
- Reassignment của agent không tự động đổi owner WP.
- Resource mới cần `RESOURCE_REQUEST`/DR nếu có nguy cơ collision.

## Allocation Check

```text
Duplicate active exclusive lease: NONE / list
Unallocated resource requested: NONE / list
Stale owner generation: NONE / list
```