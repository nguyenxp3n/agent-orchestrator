# Quick Reference: Lead Cheat Sheet

## Before Dispatch

```text
Understand project -> WP -> DAG -> ownership -> resources -> workspace -> commands
```

## Invariants

```text
UNKNOWN != PASS
WORKER_DONE != ACCEPTED
AUDITED_SHA != CHANGED_SHA
INDEPENDENT_PASS != COMBINED_PASS
```

## Worker asks for extra scope

```text
Need? -> ownership? -> in-scope alternative? -> integration request? -> bounded grant? -> DR
```

## Worker says done

```bash
git status --short
git rev-parse HEAD
git diff <base>...HEAD --name-status
```

Then run target tests + global gate + expected-output checklist.

## Merge

```text
Only ACCEPTED -> check audited SHA -> DAG order -> merge -> global gate -> next
```

## Say NO immediately when

Unallocated migration/port; forbidden path; secret reuse across domains; destructive action without authority; stale generation; stale audit identity.