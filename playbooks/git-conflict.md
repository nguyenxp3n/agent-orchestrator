# Playbook: Git Conflict

## Trigger
Sequential merge phát sinh conflict.

## Classify
- Textual conflict, semantics giống nhau → Integrator resolve.
- Approved integration hotspot → theo Integration Request.
- Public contract semantics khác → Decision Request.
- Hai WPs cùng sửa exclusive area → planning violation, không che bằng merge tay.

```bash
git status
git diff --name-only --diff-filter=U
```

## Exit Criteria
Conflict resolution traceable tới source of truth, global QA pass, resulting SHA recorded.
