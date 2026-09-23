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

Agent state and WP state are not identical. A worker crash may leave the WP recoverable.

## Assignment Generation

Each reassignment increments generation. Messages/output from an old generation are stale.

## Assurance Labels

```text
IMPLEMENTED < LOCALLY_VERIFIED < ACCEPTED < INTEGRATED < RELEASE_VERIFIED
```

Each level requires its own evidence; do not advance assurance levels through wording alone.