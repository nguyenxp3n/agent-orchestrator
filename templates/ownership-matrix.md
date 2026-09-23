# Template: Ownership Matrix

## Rules

Every active write region/resource must have a clear owner. Two `EXCLUSIVE_WRITE` scopes must not overlap. Prefer `INTEGRATION_ONLY` for shared hotspots.

| WP | Path/Pattern | Mode | Resource/Contract | Owner | Notes |
|---|---|---|---|---|---|
| | | EXCLUSIVE_WRITE | | | |
| | | SHARED_READ | | | |
| | | INTEGRATION_ONLY | | | |
| | | ALLOCATED_WRITE | | | |

## Collision Review

```text
Path overlaps found:
Semantic overlaps found:
Resolution:
DAG/contract changes required:
```

## Change Control

Every ownership transfer or scope extension must have a DR/record, effective generation, and verification impact. Do not silently modify the matrix after a worker has started.