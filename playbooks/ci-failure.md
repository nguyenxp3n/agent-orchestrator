# Playbook: CI Failure

## Trigger
Cloud CI fail, hoặc local pass nhưng cloud fail.

## Triage
So commit SHA, tool/runtime versions, environment variables/secrets, service readiness, cache, OS/filesystem, rate limits. Phân loại candidate bug / CI config / environment / external outage.

```bash
gh run list --branch <branch> --limit 10
gh run view <run-id>
```

## Forbidden Response
Không gắn nhãn flaky chỉ vì rerun pass; không sửa application code trước khi biết lỗi thuộc lớp nào.

## Exit Criteria
Root cause hoặc bounded classification có evidence; required job xanh trên đúng SHA hoặc release status vẫn blocked.
