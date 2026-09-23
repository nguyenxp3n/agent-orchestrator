# Worker Completion Checklist

Workers must verify every item prior to submitting a `WORKER_COMPLETE_CLAIM`.

## 1. Deliverable verification
- [ ] All expected files specified in the contract exist on disk in their designated paths.
- [ ] Implementation satisfies the Success Predicate completely.
- [ ] All deliverable layers (backend logic, migrations, frontend views, documentation) are present.

## 2. Boundary compliance
- [ ] `git status --short` confirms zero modifications outside `allowed_paths`.
- [ ] Untracked files and local test caches are cleaned up.
- [ ] No unassigned migration numbers, ports, or routes were created.

## 3. Test execution
- [ ] Targeted unit and component tests were executed directly in the workspace.
- [ ] Repository-level quality gates were executed directly.
- [ ] All executed commands exited with code 0.
- [ ] Command strings, exit codes, and output summaries are recorded in the completion report.