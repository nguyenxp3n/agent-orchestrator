# Seven-Step Forensic Audit Checklist

The independent Auditor executes this protocol before approving any Work Package candidate commit.

## Step 1: Git identity verification
- [ ] Inspect branch name and commit history: `git branch --show-current`, `git log -1 --stat --oneline`.
- [ ] Verify candidate commit SHA matches the commit submitted in the completion report.
- [ ] Base commit SHA matches the assigned baseline.

## Step 2: Workspace hygiene inspection
- [ ] Inspect working tree: `git status --short`.
- [ ] Verify no untracked artifacts, temporary credentials, or uncommitted modifications exist.

## Step 3: Diff boundary inspection
- [ ] Review full diff: `git diff <base>...<candidate> --name-status`, `git diff <base>...<candidate>`.
- [ ] Confirm every modified, created, or deleted file resides strictly within `allowed_paths`.
- [ ] Verify no modifications to `FORBIDDEN_PATHS` or unassigned semantic resources exist.

## Step 4: Independent targeted test execution
- [ ] Execute package-specific unit and component tests directly from the clean workspace.
- [ ] Verify commands exit with code 0. Record exact command strings and test summaries.

## Step 5: Repository-wide quality gate execution
- [ ] Execute canonical repository quality gate (`task qa`, `make test`, `pnpm test`, etc.).
- [ ] Confirm candidate introduces zero regressions against the pre-existing baseline.

## Step 6: Contract and deliverable validation
- [ ] Verify 100% of expected deliverable files exist on disk.
- [ ] Verify non-counting outcomes: ensure tests assert genuine business logic rather than empty stubs.

## Step 7: Security and contract invariants
- [ ] Verify preserved backward compatibility for public interfaces and event topics.
- [ ] Verify proper credential and secret handling.
- [ ] Sign audit report with final disposition: `ACCEPT`, `REJECT/REWORK`, or `ESCALATE`.