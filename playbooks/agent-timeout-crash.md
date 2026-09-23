# Playbook: Agent Timeout / Crash

## Trigger
Worker mất heartbeat, timeout, process crash hoặc provider session mất.

## Freeze and Capture
Không xóa workspace. Chụp `git status`, `git diff`, recent commits, leases, DRs và warnings.

```bash
git status --short
git diff
git log -3 --oneline
```

## Recovery
Phân loại clean recoverable / dirty recoverable / corrupted / unsafe-unknown. Tạo Recovery Bundle, tăng assignment generation, transfer WP resources sang worker mới. Reject output từ generation cũ.

## Exit Criteria
Worker mới nhận đúng safe commit/context/resources; zombie generation fenced; acceptance criteria còn lại rõ.
