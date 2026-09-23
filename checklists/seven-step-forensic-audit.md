# Checklist: Seven-Step Forensic Audit

## 1. Git identity / commit

```bash
git branch --show-current
git rev-parse HEAD
git log -1 --stat --oneline
```

```text
[ ] Candidate SHA matches report
[ ] Base SHA known
[ ] Commit history/convention acceptable
```

## 2. Workspace hygiene

```bash
git status --short
git ls-files --others --exclude-standard
```

```text
[ ] Dirty/untracked files explained
[ ] No credential/secret junk
```

## 3. Diff boundary

```bash
git diff <base>...HEAD --name-status
git diff <base>...HEAD
```

```text
[ ] Every changed path authorized
[ ] Renames/deletes authorized
[ ] Semantic resources authorized
```

## 4. Target/unit tests

```text
[ ] Commands taken from Project Execution Profile
[ ] Commands executed independently
[ ] Exit codes/logs captured
```

## 5. Global quality gate

```text
[ ] Canonical repo-wide command executed
[ ] Baseline failures separated from regressions
```

## 6. Barem / expected outputs

```text
[ ] Database/migration files complete
[ ] Backend complete
[ ] Frontend/mobile complete if required
[ ] Contracts/generated artifacts complete
[ ] Tests/docs complete if required
```

## 7. Contracts/security/regression + disposition

```text
[ ] Frozen contracts respected
[ ] Security invariants checked
[ ] Cross-module regression risk checked
[ ] Every acceptance criterion maps to evidence
[ ] Disposition is exactly ACCEPT / REJECT_REWORK / ESCALATE
[ ] Audit binds exact candidate SHA
```