# Template: Forensic Audit Report

## Audit Identity

```text
AUDIT_ID:
WP_ID:
Auditor:
Base SHA:
Candidate SHA:
Branch/worktree:
Audit time:
```

## Seven-Step Evidence

1. Git identity/commit evidence:
2. Workspace hygiene:
3. Diff scope/resource evidence:
4. Target/unit test evidence:
5. Global quality gate:
6. Expected outputs/barem:
7. Contracts/security/regression:

## Criteria-Evidence Matrix

| Criterion | PASS/FAIL/UNKNOWN | Evidence | Finding ID |
|---|---|---|---|
| | | | |

## Findings

```text
FINDING_ID:
Severity: CRITICAL | HIGH | MEDIUM | LOW
Category:
Evidence:
Impact:
Required corrective action:
```

## Baseline Notes

Phân biệt pre-existing failures với candidate regressions.

## Disposition

```text
ACCEPT | REJECT_REWORK | ESCALATE
Reason:
Residual risks:
Audit attestation bound to Candidate SHA:
```

Nếu candidate thay đổi sau báo cáo này, Auditor phải đánh giá lại attestation.