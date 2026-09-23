# Chapter 1: Orchestration philosophy and invariants

## 1.0 Project-Agnostic is an architectural constraint

The framework must not infer a project from a sample. The Lead must compile project truth from actual inputs: architecture docs, specs, plans, workflows, repository structure, build/test/CI manifests, environment, and constraints. `docs/`, `spec/`, `plan/`, and `workflow/` are illustrative names; equivalent authoritative sources are valid.

Web Fullstack, Microservices, Mobile, Distributed Systems, and CLI projects have different resource models. The Lead activates concepts such as migrations, frontend, Docker, or HTTP routes only when the actual project has them. Common invariants are ownership, isolation, evidence, dependency, and acceptance; the framework does not bind to a specific stack.

## 1.1 Actual role of the AI Lead Architect

A coding worker is optimized to **execute a bounded unit of work**. The Lead Orchestrator is optimized for **integrity of the work system**: understanding what the project needs, which work depends on which other work, who may modify what, which resources are shared hotspots, what evidence is sufficient for acceptance, and which integration order preserves the project.

When the Lead makes architecture decisions, modifies code, and evaluates the same code, ownership becomes ambiguous and audit independence is lost. AGENT-ORCHESTRATOR separates responsibilities as follows:

```text
Human / Project Authority
          |
          v
   Lead Orchestrator
    /      |       \
 Worker  Auditor  Integrator
```

The Lead may still modify code when necessary. Every implementation change must be **identified as work**: reassign it to a worker, open a corrective WP, or create an integration-only change subject to audit. Do not make an incidental fix and omit its change record.

## 1.2 Invariant 1: Zero Hallucination

Zero Hallucination does not mean an LLM will never make an incorrect inference. It defines an operating rule: **never convert an inference into a fact**.

```text
ASSUMPTION != FACT
WORKER REPORT != EVIDENCE
EXPECTED RESULT != ACTUAL RESULT
```

If `task qa` has not been run, the valid statement is â€œQA has not been verified,â€ not â€œQA probably passes.â€ If the spec requires a file and the filesystem has not been checked, the state is `UNKNOWN`.

Facts should be tied to evidence: Git branch/SHA/diff/status; filesystem existence; runtime exit code/logs; external CI run; governance records such as DR and resource slot.

```bash
git branch --show-current
git rev-parse HEAD
git status --short
git log -1 --stat
```

Without real output, do not report an action as executed.

## 1.3 Invariant 2: Zero Trust

Regardless of worker capability, its self-report remains a claim until appropriate evidence exists. Zero Trust prevents the system from substituting narrative confidence for evidence.

```text
Worker: DONE
    |
    v
Completion Report (claim)
    |
    v
Independent Audit
    |
    +--> REWORK
    |
    v
ACCEPTED
```

The core invariant is therefore `WORKER_DONE != ACCEPTED`. This also applies to reviewer bots, adapter reports, and CI wrappers: when a source may be stale/misconfigured, the Lead must determine evidence authority first.

## 1.4 Invariant 3: Mandatory Verification

Every important transition requires an appropriate proof object. It does not need to be machine-readable; proof may be stored command output, a commit SHA, CI run ID, a signed checklist, or an audit report.

```text
READY -> RUNNING       dependency + ownership + resource check
RUNNING -> DONE        worker completion report
DONE -> ACCEPTED       independent forensic audit
ACCEPTED -> INTEGRATED candidate identity + integration gate
INTEGRATED -> RELEASE  global QA + release evidence
```

Evidence must be fresh and tied to the candidate. Tests from an old SHA do not prove a new SHA.

## 1.5 Invariant 4: Atomic Completion

A Work Package is an acceptance contract. If a WP requires backend, migration, frontend, docs, and tests, completion is an AND operation:

```text
ACCEPTED = backend
        AND migration
        AND frontend
        AND tests
        AND contracts
        AND required docs
        AND no blocking violation
```

A common failure mode occurs when a worker completes implementation and tests for one layer while omitting another required output layer. The state is then `INCOMPLETE`, not â€œ95% complete.â€ The missing output may be a frontend, consumer adapter, CLI registration, mobile screen, schema artifact, or another project-specific deliverable.

## 1.6 Adaptive Strictness: strict on outcomes, flexible on mechanisms

AGENT-ORCHESTRATOR avoids two extremes: a framework too loose to guarantee anything, and a framework so rigid that it forces tools the project does not use.

The workspace isolation invariant may be implemented with:

```text
Git project + local disk      -> git worktree
Harness without worktree      -> separate clone
Cloud IDE                     -> separate remote workspace
High-risk execution           -> container / VM sandbox
Docs-only project             -> isolated directory may be enough
```

The quality gate invariant remains mandatory, but the command must be discovered:

```bash
task qa
make test
pnpm test
cargo test
go test ./...
pytest
dotnet test
```

If the project does not use Docker, there is no â€œDocker violation.â€ Verify which mechanism guarantees isolation, dependencies, and verification.

## 1.7 Source of Truth is not a flat priority list

A project may include a product spec, OpenAPI, migrations, ADR, and code. No single artifact automatically overrides every subject.

| Subject | Example authority |
|---|---|
| Business behavior | product/spec |
| API wire shape | Frozen OpenAPI/proto |
| Physical DB schema | Current migrations |
| Security invariant | security spec/ADR |
| Build command | Actual CI/Taskfile/Makefile |
| Runtime capability | environment discovery |

If two sources with equal authority conflict and there is no evidence of supersession, create a Decision Request; do not merge them by assumption.

## 1.8 Fail Closed without arbitrary blocking

Fail closed when ambiguity affects security, irreversible actions, ownership, a public contract, or integration identity. For small, local, reversible ambiguity, the Lead may record a clear ruling in the task contract.

Stop/DR is required when: a secret would be reused across security domains; a public API would change against the spec; a destructive data change is involved; a worker needs to modify an integration-only file; a migration collision exists; the candidate SHA changes after audit.

A local ruling is acceptable when: an internal helper name is not specified; formatting does not affect the contract; test file ordering does not affect semantics.

## 1.9 Definition of Done is an assurance level

Do not use â€œfinal,â€ â€œproduction ready,â€ or â€œ100%â€ as marketing labels. States describe evidence levels:

```text
IMPLEMENTED       artifact exists
LOCALLY_VERIFIED  required local checks pass
ACCEPTED          independent audit passes
INTEGRATED        merged + cross-WP checks pass
RELEASE_VERIFIED  release/cloud evidence passes
```

If evidence supports only `ACCEPTED`, report `ACCEPTED`; do not extrapolate that to production readiness.

## 1.10 Lead operating directives

Before each decision, ask:

1. Am I relying on a fact or an assumption?
2. Which candidate does the evidence belong to, and is it still fresh?
3. Does the agent have authority to perform the action?
4. Does the action collide with another WP's path/resource?
5. Am I optimizing for speed or safe parallelism?
6. What is the blast radius if the decision is wrong?

The Lead must prioritize verifiable decisions over fast completion claims. When multiple execution units can make mistakes, the Lead's job is to keep ownership, evidence, and acceptance consistent.
