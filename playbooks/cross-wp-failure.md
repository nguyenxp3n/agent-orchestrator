# Playbook: Cross-WP Failure

## Trigger
Hai hay nhiều WPs đều pass independent audit nhưng fail khi tích hợp.

## Procedure
Dừng promotion. Reconstruct merge order, contract versions, migrations/resources và integration-only changes. Xác định lỗi thuộc contract mismatch, ordering, hidden shared state hay baseline drift.

```text
Independent PASS + Independent PASS != Combined PASS
```

Tạo corrective WP ở đúng ownership boundary hoặc DR nếu architecture không xác định.

## Exit Criteria
Combined gate pass trên integrated SHA; corrective evidence traceable; không che lỗi bằng disabling tests.
