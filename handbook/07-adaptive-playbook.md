# Chapter 7: Adaptive handling and edge cases

## 7.1 Adaptation principle

The framework must not become a rigid checklist that breaks the project. The Lead preserves invariants while adapting mechanisms to actual evidence.

```text
Invariant stays fixed -> mechanism may change
Evidence requirement  -> command/source adapts
Ownership discipline  -> repository structure adapts
Isolation             -> worktree/clone/container/remote adapts
```

## 7.2 No Docker or insufficient local RAM

Do not force Docker. Identify the property Docker would have protected: dependency isolation, service reproducibility, DB/queue availability.

Possible replacements include:

- SQLite/in-memory DB when its semantics are sufficient for unit tests;
- local Postgres instance with a separate database/schema per worker;
- remote disposable service;
- mocks/fakes for external dependencies in unit tests;
- sequentialize tasks when the resource cannot be isolated.

```text
No Docker -> reduce concurrency if isolation cannot be proven.
```

Do not sacrifice correctness merely to keep all 10 agents running.

## 7.3 Agent timeout/crash

Do not delete the worktree immediately. Freeze it and capture:

```bash
git status --short
git diff
git log -3 --oneline
```

Record leases, pending DRs, last safe commit, and warnings. Classify the workspace as clean/dirty/corrupted/unsafe. Create a Recovery Bundle and increment the assignment generation. If the previous agent returns, treat it as a zombie; reject output from the old generation.

## 7.4 Spec conflicts with the repository

Do not arbitrarily choose “code is truth” or “spec is truth.” Determine authority by subject. For example, a build command in an old README may be superseded by current CI config; API wire shape may be governed by a frozen OpenAPI contract even when implementation has drifted.

For an equal-authority conflict:

```text
STATUS: AWAITING_DECISION
Fact A: ... evidence path
Fact B: ... evidence path
Impact: ...
Options: ...
Recommendation: ...
```

## 7.5 Git conflicts across branches

Classify the conflict:

1. Textual only, semantics agree → Integrator resolves.
2. Shared hotspot with an Integration Request → apply the request according to the frozen contract.
3. Contract semantics differ → DR.
4. Two WPs modify an area that should have been exclusive → planning violation; do not hide it with a manual merge.

```bash
git diff --ours -- <file>
git diff --theirs -- <file>
```

Resolve against the source of truth and contracts, not the “newer branch.”

## 7.6 Worker requests scope expansion

Use playbook `scope-expansion.md`. Principle: do not expand scope by default when an in-scope solution exists. If expansion is mandatory, define exact paths/resources, reason, invariants, additional verification, and explicit expiry/ownership.

## 7.7 Migration collision

If two branches claim the same allocated identifier/resource slot `R2`, stop integration. In a DB project this may be a migration number; in another project it may be a port, event/schema version, command namespace, or another semantic resource. Do not renumber/reassign before checking dependency references. Determine ownership, reassign one WP, update artifacts/references/tests, and re-audit the candidate whose identity changed.

## 7.8 CI fails while local verification passes

Do not immediately conclude “CI is flaky.” Compare commit SHA, runtime versions, env/secrets, service readiness, cache, OS/filesystem differences. If a rerun passes without a root cause, record flaky evidence and the handling policy; preserve the failure record.

## 7.9 Contract changes while downstream work is running

A change to a frozen contract makes downstream context stale. Pause affected WPs and determine compatibility. If the change is breaking, issue a new contract version/generation, refresh context, and rerun affected verification. Do not allow a worker to finish against the old contract and merge with a plan to “fix it later.”

## 7.10 Main drift after audit

A candidate audit is based on a specific base/main. Main drift can create new conflicts or regressions. The Integrator must rebuild/revalidate the combination. If the candidate commit changes, the stale audit is invalid.

## 7.11 Cross-WP failure

If two WPs pass independently but fail together, do not assign blame by intuition. Reconstruct order, contracts, resources, and the integration diff. Block promotion. Create a corrective WP at the correct ownership boundary or a DR if architecture is ambiguous.

## 7.12 Harness/tool outage

If the coding-agent provider, Git host, or CI is unavailable:

- preserve canonical state locally;
- mark the affected check `BLOCKED_ENVIRONMENT`/`UNKNOWN`, not `FAIL_IMPLEMENTATION`;
- do not consume retry budget for external failures when policy distinguishes them;
- resume from checkpoint when the service returns.

## 7.13 Safe Mode

When canonical coordination state is no longer trustworthy, such as duplicate ownership, a corrupted registry, an unknown active writer, or inconsistent migration slots, stop new dispatch and integration.

Safe Mode procedure:

```text
1. Freeze mutations
2. Snapshot Git/workspaces/resources
3. Reconstruct truth from repository + evidence
4. Resolve contradictions
5. Rebuild ownership/resource ledger
6. Revalidate affected candidates
7. Resume with new baseline/generation
```

## 7.14 Lower concurrency can be the correct decision

“5–10 agents” describes capability, not a quota. If the project has only three independent boundaries, run three agents. Artificially splitting work across 10 agents increases hotspots, coordination cost, and hallucination surface.

## 7.15 Final decision heuristic

When no playbook fits, the Lead applies this order:

```text
Protect security/irreversible data
Protect source-of-truth/contracts
Protect ownership/resources
Protect audit identity/evidence
Preserve recoverability
Then optimize speed/convenience
```

If evidence is still insufficient, the correct state is `UNKNOWN` or `AWAITING_DECISION`, not a fabricated confident answer.