# Chapter 5: Zero-Trust Forensic Audit Protocol

## 5.1 Objective

An audit answers one question: **Does the candidate commit satisfy the Work Package contract?** It ignores conversational claims and unverified assertions. Audits bind strictly to exact base and candidate commit SHAs, relying solely on freshly captured evidence.

## 5.2 Step 1: Git identity and commit inspection

```bash
git branch --show-current
git rev-parse HEAD
git log -1 --stat --oneline
```

Record the active branch name, candidate commit SHA, and base commit SHA. Verify commit messages against project conventions. If the commit SHA differs from the worker completion report, the report is stale; audit the actual candidate commit or halt for clarification.

## 5.3 Step 2: Workspace hygiene

```bash
git status --short
git ls-files --others --exclude-standard
```

Check for untracked files, stray build artifacts, lingering credentials, and uncommitted edits. A dirty working tree invalidates clean builds. Uncommitted changes must be resolved before proceeding.

## 5.4 Step 3: Diff boundary and destructive change inspection

```bash
git diff <base>...HEAD --name-status
git diff <base>...HEAD --stat
git diff <base>...HEAD
```

Verify every modified path against `allowed_paths`, `readonly_paths`, and `forbidden_paths`. Inspect renamed and deleted files individually. Verify semantic resources: database migration sequence numbers, API routes, database tables, environment variable names, network ports, and event topics.

Any modification outside assigned paths constitutes a `SCOPE_VIOLATION`. Unapproved modifications must not be accepted simply because the code appears functional.

## 5.5 Step 4: Independent targeted test execution

Execute project test commands directly rather than relying on cached outputs:

```bash
# Example targeted test commands discovered during intake
pnpm test -- review
pytest -q tests/review
cargo test review
```

Disable test caches when supported by the toolchain to avoid false positives. Record the exact command string, exit code, test counts, and failure traces.

## 5.6 Step 5: Full repository quality gate

Run canonical repository verification:

```bash
task qa
# Or project equivalents: make test / pnpm check / cargo test / go test ./... / pytest
```

Commands in this documentation are illustrative. The Auditor must execute the **canonical quality gate defined by the project**. Distinguish pre-existing baseline failures from regressions introduced by the candidate commit. Never penalize a worker for historical failures, but never conceal failing tests.

## 5.7 Step 6: Deliverable checklist and expected outputs

Passing unit tests do not prove feature completeness. Compare files on disk against the contractual deliverables:

```text
Required: <domain-implementation-artifacts>
Required: <client/consumer/integration-artifacts>
Actual: Domain files exist, consumer integration files missing
Disposition: REWORK
```

Verify migration up and down scripts, API wrappers, component exports, documentation updates, generated artifacts, and test fixtures specified in the Work Package.

## 5.8 Step 7: Contracts, security, regression, and audit reporting

Verify frozen API schemas, event contracts, database schemas, security invariants, credential handling, transaction semantics, and downstream consumer dependencies.

The final Audit Report must record:

```text
WP ID
Worker identifier and assignment generation
Branch name and worktree path
Base commit SHA and Candidate commit SHA
List of changed files
Executed test commands and process exit codes
Acceptance criteria mapping to verified evidence
Audit findings categorized by severity
Disposition: ACCEPT | REJECT/REWORK | ESCALATE
Residual risks and documented unknowns
```

## 5.9 Missing evidence is recorded as UNKNOWN

Never infer passing status from the absence of logged errors. If cloud CI is inaccessible, report:

```text
Cloud CI: UNKNOWN
Reason: Network credentials unavailable in local test harness
Local gate: PASS, exit code 0
Effect: Cloud-level release assurance unestablished
```

Depending on the package contract, an `UNKNOWN` status either blocks local acceptance or limits the final assurance level to `LOCALLY_VERIFIED`.

## 5.10 Immutable audit identity

Acceptance binds strictly to an exact candidate commit SHA. If a worker pushes subsequent changes after an audit, even for minor documentation edits, earlier approval does not transfer to the new commit.

```bash
expected=<audited-sha>
actual=$(git rev-parse HEAD)
test "$expected" = "$actual"
```

A differing commit SHA indicates a `STALE_AUDIT_SHA`, requiring a fresh audit.

## 5.11 Writing actionable REWORK reports

Never issue vague rejections. Specify:
- Exactly which acceptance criteria failed
- Verifiable command outputs demonstrating the failure
- Specific files or paths that violate boundaries
- Permitted directories where fixes may be applied
- Required test commands that must exit with code 0
- Passing components that do not require re-implementation

## 5.12 Audits verify; they do not rewrite

The Auditor does not modify source code and approve its own changes. When defects are discovered, the Auditor issues a rework directive to the worker. For small integration patches applied by the Integrator, the Auditor conducts a distinct audit on the resulting integration candidate.

## 5.13 Summary checklist

```text
1. Git identity and candidate commit verification
2. Working tree status and untracked file hygiene
3. Detailed diff inspection against assigned boundaries
4. Independent execution of targeted unit tests
5. Execution of canonical repository quality gates
6. 100% verification of expected deliverable files
7. Contract, security, and regression checks with signed disposition
```

Use the operational checklist at `checklists/seven-step-forensic-audit.md`.