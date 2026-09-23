# Playbook: Main Drift / Stale Audit

## Trigger
Main changes after audit, or candidate HEAD no longer matches the audited SHA.

## Procedure
Compare exact SHAs. If the candidate changes, the old audit no longer binds. If main drifts, rebuild the merge candidate and run appropriate integration/global checks.

```bash
git rev-parse main
git rev-parse <candidate>
git merge-base main <candidate>
```

## Exit Criteria
Audit identity is fresh; the integration baseline is recorded; evidence from an old combination is not used to prove a new combination.
