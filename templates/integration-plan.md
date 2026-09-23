# Template: Sequential Integration Plan

## Integration Batch

```text
Batch ID:
Integration branch/workspace:
Baseline main SHA:
Repository merge policy: no-ff | squash | rebase | other
Global QA command:
Cloud CI inspection method:
```

## Dependency-Aware Queue

| Order | WP | Dependency ready? | Audited SHA | Audit disposition | Integration Requests |
|---:|---|---|---|---|---|
| 1 | | | | ACCEPT | |

## Pre-Merge Checks

- candidate SHA matches audit;
- current main drift evaluated;
- dependencies integrated;
- resource/migration ordering valid;
- working tree clean.

## Merge Procedure

```bash
git switch <integration-branch>
git merge --no-ff <candidate-branch>
<global-quality-command>
```

Thay lệnh merge theo repository policy nếu khác.

## Cross-WP Gates

Build/test, contracts, migrations, critical journeys, security checks, startup/smoke.

## Failure Handling

Dừng queue tại first blocking failure; classify candidate vs integration vs environment; tạo corrective WP/DR thay vì tiếp tục merge các WP sau.

## Release Evidence

Ghi resulting SHA, included WP/audit IDs, command exit codes, CI run IDs, waivers và unverified external environments.