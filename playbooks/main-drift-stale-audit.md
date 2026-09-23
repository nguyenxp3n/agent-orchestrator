# Playbook: Main Drift / Stale Audit

## Trigger
Main thay sau audit hoặc candidate HEAD không còn bằng audited SHA.

## Procedure
So exact SHAs. Nếu candidate đổi, audit cũ không bind. Nếu main drift, rebuild merge candidate và chạy integration/global checks phù hợp.

```bash
git rev-parse main
git rev-parse <candidate>
git merge-base main <candidate>
```

## Exit Criteria
Audit identity fresh; integration baseline được ghi; không dùng evidence từ tổ hợp cũ để chứng minh tổ hợp mới.
