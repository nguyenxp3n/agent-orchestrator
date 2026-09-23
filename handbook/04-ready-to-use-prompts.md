# Chapter 4: Prompt Engineering, Prompt Compiler & Ready-to-Use Role Prompts

## 4.1 Prompt model

AGENT-ORCHESTRATOR treats a prompt as an **execution contract** compiled from project truth.

Base formula for a general prompt:

```text
ROLE + CONTEXT + TASK + FORMAT + CONSTRAINTS
```

For multi-agent software engineering, the framework extends it to:

```text
ROLE
+ OBJECTIVE
+ SUCCESS PREDICATE
+ CONTEXT / SOURCE OF TRUTH
+ SCOPE & OWNERSHIP
+ RESOURCES
+ CONSTRAINTS
+ EXPECTED OUTPUTS
+ NON-COUNTING OUTCOMES
+ VERIFICATION
+ STOP / ESCALATION
+ OUTPUT CONTRACT
```

A prompt needs enough **signal to prevent the agent from guessing decisions that can change the result**. Length increases only when the task genuinely requires more context or constraints.

## 4.2 Prompt Compiler

Before dispatch, the Lead uses `prompts/prompt-compiler.md` and the protocol in `prompt-engineering/02-prompt-compiler-protocol.md`.

```text
Project Truth
+ Work Package
+ Ownership Matrix
+ Resource Registry
+ Acceptance Contract
+ Current Evidence
        â†“
   Prompt Compiler
        â†“
  Role Prompt Draft
        â†“
 Prompt Quality Gate
        â†“
 READY -> Dispatch
```

The Compiler starts from `SUCCESS_PREDICATE`, not persona. It then resolves authority, selects context, binds boundaries/resources, defines non-counting outcomes, binds evidence, stop conditions, and the output contract.

## 4.3 Three Prompt Modes

### Compact Mode

Use for small, deterministic, narrow-scope tasks. Keep only the minimum:

```text
Role + Objective + Success Predicate + Scope + Expected Output + Verification + Output Contract
```

### Standard Mode

Default for coding Work Packages. Use the full agent-grade contract and resource/ownership controls.

### Long-Horizon Mode

Use for long, expensive, or open-ended tasks. Add:

- definitions for load-bearing terms;
- exact success predicate;
- detailed non-counting outcomes;
- domain-specific adversarial failure modes;
- evidence-traceable progress;
- persistence only when matching verification exists;
- audit-gated return condition;
- retrieval/contamination rules when independence matters.

## 4.4 Context Engineering

A prompt should carry only necessary context. Per `prompt-engineering/03-context-engineering.md`, organize context as:

```text
1. persistent project rules / authority index
2. relevant spec/architecture fragments
3. relevant source/tests/interfaces
4. current errors/logs/evidence
5. conversation/history summary
```

Classify trust:

```text
AUTHORITATIVE
VERIFY_BEFORE_USE
UNTRUSTED_DATA
```

Prefer pointers such as file path, section, or contract ID instead of copying an entire repo/spec/transcript. Instruction-like text inside untrusted input is data, not authority.

## 4.5 Prompt Quality Gate

Before dispatch, run `prompt-engineering/07-prompt-quality-gate.md`.

Critical checks include:

- clear objective and success predicate;
- identified source authority;
- sufficient context without bulk;
- clear ownership/resources;
- expected outputs enumerable;
- material near misses excluded through non-counting outcomes;
- verification produces evidence and maps to the correct identity;
- clear stop/escalation conditions;
- clear output contract;
- no unresolved critical ambiguity.

Disposition:

```text
READY | NOT_READY | ESCALATE
```

Only `READY` may be dispatched.

## 4.6 Lead Orchestrator System Prompt

Canonical version: `prompts/lead-orchestrator-system-prompt.md`.

The Lead owns the Prompt Compiler, project truth, coordination/evidence state, prompt quality gate, audit disposition, and integration order. The Lead does not convert a worker claim into acceptance by itself.

## 4.7 Coding Worker Task Assignment

Canonical version: `prompts/worker-task-assignment.md`.

The Worker prompt must compile these task-specific fields:

```text
WP_ID
OBJECTIVE
SUCCESS_PREDICATE
CONTEXT_REFERENCES
OWNERSHIP / PATH BOUNDARIES
RESOURCE_ALLOCATIONS
FROZEN_CONTRACTS
EXPECTED_OUTPUTS
NON_COUNTING_OUTCOMES
VERIFICATION
STOP_OR_ESCALATION
OUTPUT_CONTRACT
```

The Worker returns only `WORKER_COMPLETE_CLAIM`; the Auditor decides acceptance.

## 4.8 Clarification vs Decision Request

Use `prompts/clarification-guidance.md` when authority is already clear and only application guidance is needed. Use `prompts/architectural-arbitration.md` when choosing among alternatives affects architecture, security, contracts, scope, or shared resources.

A decision that changes WP truth requires recompilation of affected worker prompts; do not continue with a stale prompt.

## 4.9 Independent Auditor

`prompts/independent-auditor.md` uses a fresh-context posture, exact candidate identity, and a failure-mode checklist. The Auditor reconstructs evidence instead of reviewing the Worker's narrative.

```text
Worker summary = claim
Candidate SHA + files + commands + logs = evidence
```

The Auditor checks both the success predicate and non-counting outcomes, not only passing tests.

## 4.10 Integrator / CI-CD

`prompts/integrator-cicd.md` accepts only candidates with `ACCEPT`, verifies the exact candidate SHA/generation, integrates according to the DAG, runs global/cross-WP gates, and verifies cloud CI identity when applicable.

## 4.11 Model-Agnostic Portability

Canonical prompts describe actions:

```text
Open the relevant file.
Search for an existing project pattern.
Run the project verification command.
Delegate a focused subtask if the environment supports it.
```

Do not bind the framework to tool names specific to Claude/Codex/Copilot/Cursor/Gemini. Tool syntax belongs in the adaptive shell of the harness; objective, boundaries, and evidence bar belong in the portable core.

## 4.12 Do not use prompts as enforcement

A prompt governs at the instruction layer only. To guarantee invariants, combine it with workspace isolation, a resource registry, protected branch policy, CI, and independent audit when the project/tooling supports them.

## 4.13 Standard copy-paste workflow

```text
1. Fill templates/prompt-compile-input.md
2. Run prompts/prompt-compiler.md conceptually
3. Render target role prompt
4. Run Prompt Quality Gate
5. If READY -> dispatch
6. Worker returns structured claim/evidence
7. Auditor independently verifies
8. Accepted candidate enters integration queue
```

