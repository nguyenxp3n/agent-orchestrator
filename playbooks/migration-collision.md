# Playbook: Migration Collision

## Trigger
Hai branches dùng cùng migration ID hoặc một worker cần thêm migration chưa được cấp.

## Immediate Action
Dừng affected integration. Xác định slot owner từ Resource Registry; không tự renumber bằng cảm tính.

```bash
find migrations -maxdepth 1 -type f | sort
git diff <base>...HEAD -- migrations/
```

## Resolution
Giữ slot của owner hợp lệ; cấp slot mới cho WP khác; cập nhật filenames/references/tests; kiểm dependency ordering; candidate đổi phải re-audit.

## Exit Criteria
Không duplicate ID, dependency sequence hợp lệ, up/down semantics pass, audit mới bind đúng SHA.
