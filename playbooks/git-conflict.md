# Playbook: Git Conflict

## Trigger
A conflict occurs during sequential merge.

## Classify
- Textual conflict, same semantics → Integrator resolves.
- Approved integration hotspot → follow the Integration Request.
- Public contract semantics differ → Decision Request.
- Two WPs modify an exclusive area → planning violation; do not hide it with a manual merge.

```bash
git status
git diff --name-only --diff-filter=U
```

## Exit Criteria
Conflict resolution is traceable to the source of truth, global QA passes, and the resulting SHA is recorded.
