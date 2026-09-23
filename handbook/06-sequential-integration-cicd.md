# Chapter 6: Sequential Integration, Merge Queues, and CI/CD Verification

## 6.1 Why sequential integration is mandatory

Independent passing tests do not guarantee combined stability. Merging multiple feature branches simultaneously obscures the root cause of regressions and risks corrupting database migration sequences. Sequential integration preserves a clear causal chain of evidence:

```text
ACCEPTED WP-A -> merge -> global test suite
ACCEPTED WP-B -> merge -> global test suite
ACCEPTED WP-C -> merge -> global test suite
```

Batch merges are permitted only when packages are strictly orthogonal and project policy explicitly authorizes them. Merging all branches at once simply because their local tests passed is prohibited.

## 6.2 Integration ordering governed by the DAG

Branches are merged according to the dependency graph, never by worker completion speed. If Work Package B depends on interface contracts or database tables created by Work Package A, Package A must be integrated first, even if Package B finishes earlier.

```text
WP-CONTRACT -> WP-BACKEND -> WP-E2E
            -> WP-FRONTEND -> WP-E2E
```

The integration queue tracks dependency prerequisites and exact audit commit SHAs.

## 6.3 Pre-merge verification

Before executing any merge:

```bash
git fetch --all --prune
git status --short
git rev-parse HEAD
# Verify candidate commit SHA matches audited commit SHA
# Verify current main matches integration baseline
```

If the main branch has advanced since the candidate was audited, assess the impact. Rebasing or merging main into the candidate generates a new commit identity that requires revalidation.

## 6.4 Non-fast-forward merges

When repository standards maintain explicit branch history:

```bash
git switch main
git pull --ff-only
git merge --no-ff feat/wp-210 -m "merge: integrate WP-210 review backend"
```

The `--no-ff` flag is an operational preference rather than an absolute rule; repositories may enforce squash or rebase policies. The fundamental requirement is traceability: the merged change must link directly to the Work Package, verified audit logs, and candidate commit SHA. When squash merges are mandated, record the mapping from the original branch commit to the squashed commit SHA.

## 6.5 Managing shared integration hotspots

The Integrator holds restricted authority to apply approved Integration Requests to centralized routing tables, bootstrap manifests, or central dependency injection containers. The Integrator never uses this authority to perform arbitrary refactoring.

When conflicts consist of simple textual overlaps with clear semantics governed by frozen contracts, the Integrator resolves the conflict and executes verification tests. If semantic contradictions arise, the Integrator halts and submits a Decision Request.

## 6.6 Cross-Package verification gate

Following every merge or verified batch, execute:
- Canonical build, lint, typecheck, and test commands
- Schema and event contract validations
- Database migration sequence checks and upgrade/downgrade cycles
- Critical user journey end-to-end tests
- Application smoke tests and health checks
- Security scanning tools

```bash
<project-global-quality-command>
<contract-validation-command>
<critical-e2e-command>
```

Local success in an isolated worktree does not guarantee repository-wide integration success.

## 6.7 Pull request governance

When using platforms such as GitHub, GitLab, or Bitbucket, include:

```text
WP ID and package objective
Prerequisite dependencies
Audited candidate commit SHA
Audit report link and final disposition
List of modified paths and semantic resources
Verification commands and exit codes
Required integration actions
Documented risks or temporary waivers
```

A pull request provides a clean audit surface; it does not substitute for local test execution or automated CI pipelines.

## 6.8 Cloud CI/CD verification

Verify automated pipeline runs directly rather than assuming success:

```bash
gh run list --branch <branch> --limit 10
gh run view <run-id>
```

For GitLab or Bitbucket, use equivalent API or CLI tools. Record the pipeline run ID, commit SHA, completed jobs, and final outcome. Confirm that the pipeline run corresponds exactly to the commit being merged.

## 6.9 Diagnosing cloud pipeline failures

Common discrepancies between local and cloud environments:
- Stale dependency caches on CI runners
- Divergent toolchain versions or environment wrappers
- Case-sensitive filesystem behavior differing from local machines
- Missing pipeline secrets or incorrect permissions
- Dependent service containers not yet ready
- Network timeouts or API rate limits
- Architecture-specific failures in multi-platform build matrixes

Isolate failures systematically:
1. Is this a candidate source defect?
2. Is this a runner environment or tooling defect?
3. Is this a CI pipeline configuration defect?
4. Is this caused by stale runner cache?
5. Is this an external upstream outage?

Never modify application source code to bypass unclassified infrastructure failures.

## 6.10 Main branch drift

When the main branch advances between audit approval and integration execution:

```text
AUDITED CANDIDATE + PREVIOUS MAIN
             |
       BRANCH DRIFT
             v
   NEW CURRENT MAIN
```

The Lead recomputes the integration candidate, merges or rebases against updated main, and re-executes verification gates. An audit log from an earlier base commit cannot validate the merged outcome.

## 6.11 Migration integration

Pre-allocated resource slots prevent duplicate migration numbers, but semantic dependencies must still be validated. In database projects, migration N+1 may depend on tables created by migration N. In distributed systems, protocol schema R2 may depend on contracts established in R1. The dependency DAG must dictate merge ordering; unique filenames do not substitute for semantic sequence.

## 6.12 Release promotion and deployment verification

Following final integration, record:

```text
Main branch commit SHA
Included Work Package IDs
Associated audit report references
Global quality gate commands and exit codes
CI pipeline run IDs
Schema migration verification results
Active waivers and documented limitations
Unverified external deployment environments
```

Apply the `RELEASE_VERIFIED` status only after deployment-specific checks pass. Passing local test suites does not equate to verified production deployment.