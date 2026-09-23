# Chapter 6: Sequential integration and cloud CI/CD

## 6.1 Why Sequential Integration

Multiple WPs passing independently does not prove they pass together. Bulk merging destroys the ability to identify which candidate caused a regression and can violate migration/contract ordering. Sequential integration preserves causal evidence.

```text
ACCEPTED WP-A -> merge -> global gate
ACCEPTED WP-B -> merge -> global gate
ACCEPTED WP-C -> merge -> global gate
```

Batching is acceptable when WPs are fully independent and project policy allows it, but “merge everything because it is green” is not the default.

## 6.2 Integration order by DAG

Do not merge by worker completion time. If WP-B depends on contract/data from WP-A, A must integrate first even if B reaches `ACCEPTED` earlier.

```text
WP-CONTRACT -> WP-BACKEND -> WP-E2E
            -> WP-FRONTEND -> WP-E2E
```

The integration queue stores dependency readiness and candidate audit identity.

## 6.3 Pre-merge gate

Before merge:

```bash
git fetch --all --prune
git status --short
git rev-parse HEAD
# compare candidate SHA with audited SHA
# compare current main with integration baseline
```

If main drifts from the baseline after audit, assess impact: rebasing or merging main into the candidate creates a new candidate identity and usually requires revalidation.

## 6.4 Merge `--no-ff`

When project policy requires preserving WP branch history:

```bash
git switch main
git pull --ff-only
git merge --no-ff feat/wp-210 -m "merge: integrate WP-210 review backend"
```

`--no-ff` is not an absolute invariant; a repository may use squash or rebase merge. The invariant is traceability from integration back to the WP, audit evidence, and candidate identity. If repository policy requires squash, record the mapping from old commit → merge commit.

## 6.5 Shared hotspot resolution

The Integrator has narrow authority to apply approved Integration Requests to router/bootstrap/root config. Do not use that authority for incidental domain refactoring.

If a conflict is purely textual and semantics are clear from a frozen contract, the Integrator resolves and verifies it. If semantics conflict, create a DR.

## 6.6 Cross-WP Gate

After each merge or safe batch, run:

- canonical build/lint/typecheck/test;
- contract/schema/event validation;
- migration ordering + upgrade/downgrade when required by the project;
- critical E2E journeys;
- smoke/startup checks;
- applicable security checks.

```bash
<project-global-quality-command>
<contract-validation-command>
<critical-e2e-command>
```

Independent WP success is not project success.

## 6.7 Pull Request workflow

When using GitHub/GitLab/Bitbucket, a PR/MR should include:

```text
WP ID + objective
Dependencies
Candidate SHA
Audit disposition + evidence location
Changed paths/resources
Verification commands + results
Integration requests
Known risks/waivers
```

A PR is an audit surface, not evidence that replaces local/CI checks.

## 6.8 Cloud CI/CD verification

Do not trust “GitHub Actions is green” when direct verification is possible. GitHub CLI example:

```bash
gh run list --branch <branch> --limit 10
gh run view <run-id>
```

GitLab/Bitbucket use the corresponding API/CLI. Record run ID, commit SHA, jobs, and conclusion. Ensure the CI run actually belongs to the candidate being merged.

A field case used this exact pattern to verify cloud jobs directly instead of relying on a worker report. The provider does not change the invariant: the run must bind to the exact candidate identity.

## 6.9 Cloud runner issues

Common failures:

- cache contains an old dependency;
- wrapper/shim differs from local;
- case-sensitive filesystem differs;
- missing secret/permission;
- service container is not ready;
- network/rate limit;
- only one platform in a matrix job fails.

Handle by layer:

```text
candidate bug?
environment/tooling bug?
CI config bug?
stale cache?
external outage?
```

Do not “fix code” for an infrastructure failure before classification.

## 6.10 Main drift

If main changes between audit and integration:

```text
AUDITED CANDIDATE + OLD MAIN
            X
CURRENT MAIN differs
```

The Lead must recompute the merge candidate and run the appropriate gate. Do not use the old audit as evidence for the new combination.

## 6.11 Migration integration

Allocated resource slots prevent duplicate identifiers, but integration must still verify dependency semantics. In a DB project, migration N+1 may depend on schema created by migration N; in an event/proto/CLI project, resource R2 may depend on contract R1. The DAG must represent that dependency; distinct identifiers do not replace semantic ordering.

## 6.12 Promotion and release evidence

After final integration, record:

```text
main/release SHA
included WP IDs
included audit IDs/reports
global QA commands + exit codes
CI run IDs
migration/schema result
known waivers
unverified external environments
```

Use `RELEASE_VERIFIED` only when release-specific evidence exists. Local green status does not equal production verification.