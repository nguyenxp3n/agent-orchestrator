# Template: Ownership Matrix

## Rules

Mỗi active write region/resource phải có owner rõ. Hai `EXCLUSIVE_WRITE` scopes không overlap. Shared hotspots ưu tiên `INTEGRATION_ONLY`.

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

Mọi ownership transfer hoặc scope extension phải có DR/record, effective generation và verification impact. Không chỉnh matrix âm thầm sau khi worker đã chạy.