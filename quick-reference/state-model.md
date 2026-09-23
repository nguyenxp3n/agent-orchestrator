# Quick Reference: State Model

## Work Package States

```text
DRAFT -> READY -> ASSIGNED -> RUNNING
                    |          |
                    |          +-> BLOCKED
                    |          +-> AWAITING_DECISION
                    |          +-> READY_FOR_AUDIT
                    |                    |
                    |                    +-> REWORK -> RUNNING
                    |                    +-> ACCEPTED -> INTEGRATING -> INTEGRATED
                    +---------------------> FAILED/CANCELLED when explicitly decided
```

## Agent Runtime States

```text
READY -> RUNNING -> SUSPECTED_STALLED -> PAUSED/FAILED/REASSIGNED
```

Agent state và WP state không đồng nhất. Worker crash có thể để WP recoverable.

## Assignment Generation

Mỗi reassignment tăng generation. Message/output từ generation cũ là stale.

## Assurance Labels

```text
IMPLEMENTED < LOCALLY_VERIFIED < ACCEPTED < INTEGRATED < RELEASE_VERIFIED
```

Mỗi mức cần evidence riêng; không tự nhảy mức bằng ngôn từ.