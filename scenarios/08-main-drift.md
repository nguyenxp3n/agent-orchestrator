# Scenario: Main Branch Drift After Audit

## Trigger
The main branch advances with new commits while an accepted candidate branch is awaiting integration.

## Risk
Latent regressions introduced by interactions with newly merged code.

## Evidence
`git log <base>..origin/main` shows new commits since the candidate baseline.

## Immediate Action
Mark earlier audit as `STALE_AUDIT_SHA`. Block direct merge.

## Forbidden Response
Never merge an audited candidate into an advanced main branch without re-verification.

## Recovery Procedure
Rebase candidate onto updated main or merge main into the candidate branch. Execute test gates against the new commit SHA.

## Exit Criteria
New candidate commit SHA passes forensic re-audit and enters the merge queue.

## Example Lead Response
```text
Main drifted by 2 commits. Rebasing feat/wp-210. Earlier audit invalidated. Re-executing targeted test suite against new SHA.
```