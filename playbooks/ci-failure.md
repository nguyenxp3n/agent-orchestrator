# Playbook: CI Failure

## Trigger
Cloud CI fails, or local passes while cloud fails.

## Triage
Compare commit SHA, tool/runtime versions, environment variables/secrets, service readiness, cache, OS/filesystem, and rate limits. Classify as candidate bug / CI config / environment / external outage.

```bash
gh run list --branch <branch> --limit 10
gh run view <run-id>
```

## Forbidden Response
Do not label a failure flaky merely because a rerun passes; do not modify application code before identifying the failing layer.

## Exit Criteria
Root cause or bounded classification has evidence; the required job is green on the correct SHA or release status remains blocked.
