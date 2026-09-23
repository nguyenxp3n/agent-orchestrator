# Playbook: Main Branch Drift and Stale Audits

## Trigger
The main branch advances after a candidate branch has been audited and approved.

## Procedure
1. **Identify drift extent**: Inspect new commits added to main since the candidate baseline:
   ```bash
   git log <base>..origin/main --oneline
   ```
2. **Rebase or merge main**: Rebase the candidate branch onto updated main or merge main into the candidate.
3. **Invalidate earlier audit**: Because the commit SHA has changed, the earlier audit report is now marked `STALE_AUDIT_SHA`.
4. **Re-execute verification**: Execute quality gates against the new candidate commit SHA. Conduct an abbreviated audit to re-verify acceptance.