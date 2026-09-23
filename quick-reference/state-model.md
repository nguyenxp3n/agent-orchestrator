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
                    +---------------------> FAILED/CANCELLED (Explicit ruling)
```

## Agent Runtime States

```text
READY -> RUNNING -> SUSPECTED_STALLED -> PAUSED / FAILED / REASSIGNED
```

Agent runtime states and Work Package states are decoupled. A crashed worker leaves the Work Package recoverable.

## Assignment Generation Counter

Every reassignment increments the generation counter (`ASSIGNMENT_GENERATION`). Submissions matching earlier generations are discarded as stale.

## Assurance Levels

```text
IMPLEMENTED < LOCALLY_VERIFIED < ACCEPTED < INTEGRATED < RELEASE_VERIFIED
```

Every level requires distinct verified evidence. Never escalate assurance levels through conversational claims.