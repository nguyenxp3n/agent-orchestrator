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

Replace the merge command when repository policy differs.

## Cross-WP Gates

Build/test, contracts, migrations, critical journeys, security checks, startup/smoke.

## Failure Handling

Stop the queue at the first blocking failure; classify candidate vs integration vs environment; create a corrective WP/DR instead of continuing to merge later WP candidates.

## Release Evidence

Record the resulting SHA, included WP/audit IDs, command exit codes, CI run IDs, waivers, and unverified external environments.