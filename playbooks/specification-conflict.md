# Playbook: Specification Conflict

## Trigger
Hai nguồn authoritative hoặc spec và repository evidence mâu thuẫn.

## Procedure
Xác định subject: business, API, DB, security, build, deployment. Áp authority theo subject. Nếu nguồn ngang quyền và không có supersession evidence, tạo Decision Request.

```text
FACT A + source
FACT B + source
SUBJECT AUTHORITY
IMPACT
OPTIONS
```

## Forbidden Response
Không chọn “code luôn đúng” hoặc “spec luôn đúng” như luật chung; không blend hai semantics thành giải pháp thứ ba.

## Exit Criteria
Decision có authority, downstream context được refresh và affected candidate được reverified.
