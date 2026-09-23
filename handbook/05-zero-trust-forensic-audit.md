# Chapter 5: Zero-Trust Forensic Audit

## 5.1 Objective

An audit answers one question: **does the current candidate actually satisfy the Work Package contract?** It does not ask whether the worker is confident or whether the worker's tests “seem” to pass. Audit must bind to the exact base/head identity and use fresh evidence.

## 5.2 Step 1: Git identity and commit

```bash
git branch --show-current
git rev-parse HEAD
git log -1 --stat --oneline
```

Record branch, HEAD SHA, and base SHA. Check the commit message against project convention when one exists. If the candidate SHA differs from the SHA reported by the worker, the worker report is stale; audit the actual candidate or request clarification.

## 5.3 Step 2: Workspace hygiene

```bash
git status --short
git ls-files --others --exclude-standard
```

Check untracked files, generated junk, local credentials, and dirty changes after commit. A dirty working tree is not always an automatic failure, but it must be explained and bound to the correct candidate; for a release candidate, fail closed in most cases.

## 5.4 Step 3: Diff boundary and destructive changes

```bash
git diff <base>...HEAD --name-status
git diff <base>...HEAD --stat
git diff <base>...HEAD
```

Compare every path against `allowed_paths`, `readonly_paths`, and `forbidden_paths`. Check renames/deletes separately. Then inspect semantic resources: migration number, route, table, env var, port, event name.

An out-of-scope write is a `SCOPE_VIOLATION`; do not waive it because the code is “useful.”

## 5.5 Step 4: Independent unit/target tests

Use commands from the Project Execution Profile. Do not guess. Examples:

```bash
<project-specific targeted test command discovered during intake>
pnpm test -- review
pytest -q tests/review
cargo test review
```

Prefer disabling cache when the framework/toolchain supports it to avoid stale evidence. Record command, exit code, test count, and failure summary.

## 5.6 Step 5: Repo-wide quality gate

Run the project's canonical gate:

```bash
task qa
# hoặc make test / pnpm check / cargo test / go test ./... / pytest / dotnet test
```

Commands in this documentation are examples only. The Auditor must run the **quality gate the project treats as canonical**, while separating pre-existing baseline failures from candidate regressions. Do not attribute old failures to the worker, and do not hide any failure.

## 5.7 Step 6: Rubric / Expected Outputs

This layer cannot usually be replaced by green tests. Count expected files/artifacts against the contract.

Project-neutral example:

```text
Required: <domain-implementation-artifacts>
Required: <client/consumer/integration-artifacts>
Actual: first group exists, second required group missing
Disposition: REWORK
```

Check migration up/down, API wrapper, component/styles/barrel export, docs, generated artifact, fixtures, and other WP-specific outputs.

## 5.8 Step 7: Contracts, security, regression, and audit report

Check frozen API/event/schema contracts; security invariants; secret handling; idempotency/transactions when required by the WP; regression risk; downstream consumers.

The Audit Report must include:

```text
WP ID
Agent / generation
Branch / worktree
Base SHA / Candidate SHA
Changed files
Commands + exit codes
Acceptance criterion -> evidence mapping
Findings with severity
Disposition: ACCEPT | REJECT/REWORK | ESCALATE
Residual risks / unknowns
```

## 5.9 Missing evidence = UNKNOWN

Do not infer PASS from “no issue found.” If cloud CI is inaccessible, record:

```text
Cloud CI: UNKNOWN
Reason: credential/network unavailable
Local gate: PASS, exit 0
Effect: release/cloud assurance not established
```

Depending on the WP, an unknown may block `ACCEPTED` or only block `RELEASE_VERIFIED`.

## 5.10 Immutable audit identity

Acceptance must bind to the exact candidate SHA/snapshot. After audit, if the worker adds a commit even if it “only changes README,” the old audit does not automatically apply to the new SHA.

```bash
expected=<audited-sha>
actual=$(git rev-parse HEAD)
test "$expected" = "$actual"
```

Different SHA → `STALE_AUDIT_SHA`; re-audit at the appropriate scope.

## 5.11 Effective REWORK reports

Do not say “not good enough.” State exactly:

- which criterion failed;
- which evidence proves the failure;
- which file/path is missing or violates scope;
- where the worker is allowed to make corrections;
- which command must be rerun;
- which passing work does not need to be repeated when candidate identity/recovery rules permit.

## 5.12 Audit is not rewrite

The Auditor does not modify code in the same task and then sign its own acceptance. When a defect is found, the Auditor creates corrective work for the Worker. For an integration-only fix applied by the Integrator, the Auditor must audit the new candidate or apply the defined integration gate.

## 5.13 Seven-Step Checklist summary

```text
1. Git identity/commit
2. git status + untracked hygiene
3. git diff + scope/resource boundaries
4. independent unit/target tests
5. repository quality gate
6. 100% barem/expected outputs
7. contracts/security/regression + signed disposition
```

The operational checklist is in `checklists/seven-step-forensic-audit.md`.