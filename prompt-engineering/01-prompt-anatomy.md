# 01: Prompt Anatomy: from the 5-part Prompt to the Agent Execution Contract

## 1. Base formula

A general prompt can start with five components:

```text
ROLE + CONTEXT + TASK + FORMAT + CONSTRAINTS
```

- Role: what role the agent performs and how much authority it has.
- Context: the background facts required to understand the task.
- Task: the result that must be produced.
- Format: the required output shape.
- Constraints: boundaries that must not be crossed.

This formula works for short tasks. For coding agents or multi-agent orchestration, it is insufficient to prevent scope drift, false completion, and resource collisions.

## 2. Agent-grade contract

AGENT-ORCHESTRATOR extends it to:

```text
ROLE
+ OBJECTIVE
+ SUCCESS PREDICATE
+ CONTEXT
+ INPUTS / SOURCE OF TRUTH
+ SCOPE & OWNERSHIP
+ RESOURCES
+ CONSTRAINTS
+ EXPECTED OUTPUTS
+ NON-COUNTING OUTCOMES
+ VERIFICATION
+ STOP / ESCALATION CONDITIONS
+ OUTPUT CONTRACT
```

### Role

Role must define **responsibility and authority boundaries**, not a decorative persona.

Weak:

```text
You are a skilled senior developer.
```

Strong:

```text
You are the Scoped Coding Worker for WP-210. Implement only artifacts within the assigned ownership envelope; do not self-merge, expand scope, or self-declare ACCEPTED.
```

### Objective

Describe an outcome, not vague activity.

```text
OBJECTIVE: create an idempotent endpoint that satisfies the frozen contract and includes a regression test proving that a duplicate request does not create a second record.
```

### Success Predicate

A **Success Predicate** is a verifiable condition that distinguishes actual DONE from â€œlooks done.â€

```text
SUCCESS_PREDICATE:
- expected artifact exists;
- contract tests pass;
- duplicate replay test pass;
- diff stays within allowed_paths;
- no resource outside the allocation is used.
```

### Non-Counting Outcomes

List common near misses that the agent might return instead of the required result.

```text
NON_COUNTING_OUTCOMES:
- only backend is implemented while the WP requires a client artifact;
- unit tests pass but the canonical quality gate has not run;
- a solution is described instead of producing the artifact;
- â€œblocked by specâ€ is reported even though the spec is already resolved;
- an out-of-ownership workaround is used to make tests green.
```

### Verification

Verification must produce evidence:

```text
command -> exit code -> artifact/log -> criterion
```

Do not accept:

```text
"I checked everything and it is fine."
```

## 3. Prompt altitude

A prompt written at too low an altitude hard-codes every action and reduces agent adaptability. A prompt written too high is ambiguous.

The framework uses an **altitude heuristic**:

- lock outcomes, invariants, boundaries, and evidence;
- prescribe procedure only at risk-sensitive steps;
- leave implementation details to the Worker within the allowed envelope.

## 4. Positive instructions first, prohibitions second

Prefer instructions that state **what the agent must do**. Use `FORBIDDEN`/`DO NOT` for true invariants such as protected branches, secrets, destructive actions, and forbidden paths.

## 5. Structure by complexity

- Small task: short Markdown headers are sufficient.
- Complex task: use clear sections such as `OBJECTIVE`, `CONTEXT`, `BOUNDARIES`, `VERIFICATION`, `OUTPUT`.
- If multiple data layers can be confused, XML-like tags may be used, but this is a presentation choice, not an invariant.

## 6. Final rule

The best prompt is not the longest prompt. It contains **enough signal that the agent does not need to guess decisions that can invalidate the result**.

