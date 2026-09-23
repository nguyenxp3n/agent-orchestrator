# Playbook: Scope Expansion

## Trigger
Worker cần sửa path/resource ngoài WP để đạt acceptance.

## Decision Procedure
1. Chứng minh thay đổi thực sự bắt buộc, không chỉ tiện lợi.
2. Kiểm Ownership Matrix và active workers.
3. Tìm phương án trong scope hoặc Integration Request trước.
4. Nếu buộc mở scope, grant exact path/resource, không wildcard rộng.
5. Ghi invariants và extra verification.

```text
GRANT: Taskfile.yml only
PRESERVE: all existing tasks
VERIFY: task qa + task spec:validate
NO unrelated refactor
```

## Reject When
Scope thuộc WP khác, shared hotspot đang active, worker muốn tự cấp migration/port, hoặc change phá frozen contract.

## Exit Criteria
Decision record tồn tại; ownership/resource matrix cập nhật; affected workers/context được refresh; audit biết scope mới.
