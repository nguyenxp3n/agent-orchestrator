# Example: Web Fullstack với 6 Agents

## Mục tiêu

Triển khai feature set gồm contracts, auth/backend, review/backend, review/frontend, infra và CI mà không để workers cùng sửa hotspot.

## DAG

```text
WP-100 Contract Freeze
  +--> WP-210 Auth Backend --------+
  +--> WP-220 Review Backend ------+--> WP-600 Integration/E2E
  +--> WP-230 Review Frontend -----+
WP-300 Infra ----------------------+ 
WP-400 CI -------------------------+
```

## Allocation

```text
A1 WP-210: services/auth/**, migration 000021
A2 WP-220: internal/review/**, migration 000022
A3 WP-230: apps/web/src/features/review/**
A4 WP-300: deploy/**, no migration
A5 WP-400: .github/workflows/** only
A6 Auditor: read-only + command execution, no candidate edits
```

Root router và Taskfile là `INTEGRATION_ONLY`.

## Dispatch Wave

Sau WP-100 accepted/frozen, A1–A5 có thể chạy song song nếu resource checks pass. Auditor xử lý candidates khi từng worker báo completion.

## Integration

```text
merge WP-210 -> global QA
merge WP-220 -> global QA
merge WP-230 + approved route integration -> global QA/E2E
merge Infra/CI according to dependency -> cloud CI verify
```

Nếu frontend thiếu expected component dù backend pass, WP-230 không được accepted.