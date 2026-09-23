# Chapter 7: Adaptive Playbooks and Edge-Case Recovery

## 7.1 Principles of adaptation

The framework provides rigorous invariants without imposing rigid, unworkable workflows. The Lead maintains non-negotiable guarantees while adapting operational mechanisms to repository realities:

```text
Core invariants stay fixed   -> Implementation mechanisms adapt
Evidence requirement        -> Discovery commands adapt
Ownership boundaries       -> Directory structures adapt
Workspace isolation         -> Worktrees, clones, or containers adapt
```

## 7.2 Environments without Docker or with limited hardware

Never force Docker onto a project that does not use it. Determine the underlying requirements Docker would normally satisfy: dependency isolation, environment reproducibility, or database availability.

Practical alternatives:
- SQLite or in-memory databases when semantics suffice for unit test suites
- Dedicated schemas or databases per worker within a single local Postgres instance
- Remote disposable test databases or mock service stubs
- In-memory service fakes for external third-party dependencies
- Serialized execution when resources cannot be isolated

```text
If isolation cannot be proven on local hardware, reduce parallel concurrency.
```

Never sacrifice operational correctness merely to keep ten agents running concurrently.

## 7.3 Agent timeout or process crash

When a worker crashes or times out, preserve its workspace rather than deleting it immediately. Capture the working state:

```bash
git status --short
git diff
git log -3 --oneline
```

Record active resource locks, pending Decision Requests, the last known safe commit, and error logs. Classify the state as clean, dirty, or corrupted. Package the context into a Recovery Bundle, increment the assignment generation number, and assign a fresh worker. If the crashed agent subsequently resumes, its outdated generation identifier causes its outputs to be rejected as stale.

## 7.4 Specification conflicts with repository reality

When specifications contradict existing code, avoid arbitrarily declaring that either the specification or the codebase is correct. Identify the governing authority for the specific technical domain. For example, build commands in an outdated README are superseded by an active CI configuration, whereas API wire schemas defined in a frozen OpenAPI specification take precedence over diverging controller implementations.

When documents of equal authority conflict:

```text
STATUS: AWAITING_DECISION
Fact A: ... supporting evidence path
Fact B: ... supporting evidence path
Impact: ...
Technical Options: ...
Recommendation: ...
```

## 7.5 Resolving Git merge conflicts

Classify conflicts before taking action:
1. Purely textual overlap with identical semantics: Integrator resolves directly.
2. Integration hotspot with approved Integration Request: Integrator applies approved change.
3. Divergent interface semantics: Halt and submit a Decision Request.
4. Two workers modifying the same exclusive boundary: Planning defect; resolve via architectural re-alignment rather than manual patching.

```bash
git diff --ours -- <file>
git diff --theirs -- <file>
```

Resolve conflicts based on frozen contracts and documented authority, never by assuming that the newer branch is correct.

## 7.6 Worker requests for scope expansion

Reference `playbooks/scope-expansion.md`. Default to rejection whenever an alternative within the assigned scope exists. When expansion is strictly necessary, define exact paths, specific rationale, invariant boundaries, required test verifications, and explicit expiration conditions.

## 7.7 Sequential identifier collisions

If two branches claim the same allocated resource identifier (such as migration number `R2`), halt integration immediately. This applies to database migration scripts, network port allocations, API route namespaces, and event topic names. Never renumber scripts without checking cross-references. Determine ownership, reassign the colliding package, update references and tests, and conduct a fresh audit on the modified candidate commit.

## 7.8 Cloud CI failures with passing local tests

Never dismiss CI failures as temporary flakiness without investigation. Compare commit SHAs, runtime tool versions, environment variables, runner permissions, service readiness, and filesystem case-sensitivity. When a rerun passes without code changes, document the transient failure and record the incident for investigation.

## 7.9 Contract changes during downstream execution

Modifying a frozen interface invalidates in-progress downstream work. Halt affected workers immediately and assess backward compatibility. If the change is breaking, publish a new contract version, increment assignment generations, update worker contexts, and re-execute verification suites. Never permit workers to finish work against deprecated interfaces with plans to fix discrepancies later.

## 7.10 Handling main branch drift

Audits evaluate a candidate commit relative to a specific base commit. When the main branch advances, changes can introduce latent regressions. The Integrator rebases or merges the candidate with the updated main branch and executes verification suites. If the candidate commit SHA changes, the earlier audit is superseded.

## 7.11 Multi-package integration failures

When two packages pass individually but fail upon combination, avoid subjective blame. Reconstruct execution order, contract assumptions, and diffs across both branches. Block promotion to main. Construct a corrective Work Package assigned to the appropriate boundary, or file a Decision Request if architectural ambiguities exist.

## 7.12 External harness or service outages

When LLM providers, Git hosting services, or CI platforms suffer outages:
- Preserve canonical state in the local repository
- Mark affected quality gates as `BLOCKED_ENVIRONMENT`, not `FAIL_IMPLEMENTATION`
- Avoid consuming retry budgets on external infrastructure failures
- Resume execution from the last valid checkpoint once services recover

## 7.13 Safe Mode protocol

When coordination state becomes untrustworthy (unclear write boundaries, corrupted registries, unidentified active writers, or conflicting migration slots), halt new dispatches and integration runs immediately.

Safe Mode procedure:
1. Freeze all write operations across active worktrees.
2. Snapshot Git branches, workspaces, and resource allocations.
3. Reconstruct ground truth directly from repository files and commit logs.
4. Resolve discrepancies through explicit rulings.
5. Rebuild ownership ledgers and resource registries.
6. Re-audit affected candidate commits.
7. Resume operations under a newly established baseline and generation counter.

## 7.14 Reducing concurrency is an engineering decision

Coordinating five to ten agents represents capacity, not a mandatory quota. When a project presents only three orthogonal boundaries, running three agents is the correct engineering decision. Forcing artificial decomposition increases coordination overhead, hotspot contention, and error rates.

## 7.15 Final decision heuristic

When encountering unmapped edge cases, prioritize in order:

```text
1. Protect security invariants and irreversible data
2. Protect authoritative specifications and frozen contracts
3. Protect ownership boundaries and resource allocations
4. Protect audit integrity and verified evidence
5. Preserve system recoverability
6. Optimize for delivery speed and developer convenience
```

When evidence remains insufficient, the valid operational state is `UNKNOWN` or `AWAITING_DECISION`. Never fabricate confident assumptions.