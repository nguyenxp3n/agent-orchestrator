# 01: Prompt Anatomy: From 5-Part Prompts to Agent Execution Contracts

## 1. Baseline prompt formula

A routine task prompt often begins with five standard components:

```text
ROLE + CONTEXT + TASK + FORMAT + CONSTRAINTS
```

- Role: The persona and operational scope assigned to the agent.
- Context: Background information required to interpret the task.
- Task: The deliverable output the agent must produce.
- Format: The expected structure of the return payload.
- Constraints: Boundaries that must not be violated.

This formula works for small, self-contained tasks. In multi-agent software engineering, however, it fails to prevent scope drift, false completion claims, and resource collisions.

## 2. Agent-grade execution contract

Agent Orchestrator expands the baseline formula into an enforceable execution contract:

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

A role must define **operational responsibilities and authority limits**, not decorative personas.

Weak:
```text
You are a brilliant senior full-stack developer.
```

Strong:
```text
You are the Scoped Coding Worker for WP-210. You implement artifacts strictly within your assigned ownership envelope. You do not merge code, you do not expand scope, and you never mark tasks as ACCEPTED.
```

### Objective

Define outcomes directly rather than describing generic activity:

```text
OBJECTIVE: Implement an idempotent endpoint satisfying the frozen contract, accompanied by regression tests proving duplicate requests do not create duplicate database records.
```

### Success Predicate

The **Success Predicate** provides a verifiable boolean condition distinguishing genuine completion from conversational near-misses:

```text
SUCCESS_PREDICATE:
- Expected deliverable files exist on disk
- Contract test suite passes
- Replay test for duplicate requests passes
- Git diff strictly respects allowed_paths
- No unallocated semantic resources are consumed
```

### Non-Counting Outcomes

List plausible near-misses that workers frequently substitute for actual technical delivery:

```text
NON_COUNTING_OUTCOMES:
- Implementing backend logic while omitting required client integration artifacts
- Passing unit tests while failing to run the repository quality gate
- Describing a theoretical implementation instead of writing code files to disk
- Claiming blockage by specifications that have already been resolved
- Employing out-of-scope workarounds to force test suites to pass
```

### Verification

Verification requires recorded evidence:

```text
command -> exit code -> artifact/log trace -> acceptance criterion
```

Never accept conversational claims such as:
```text
"I tested the implementation and verified that everything works properly."
```

## 3. Heuristic prompt altitude

Prompts that are too low-level hard-code every keystroke, eliminating an agent's ability to navigate minor obstacles. Prompts that are too high-level invite unconstrained guessing.

The framework enforces **heuristic altitude**:
- Lock outcomes, invariants, path boundaries, and verification criteria rigidly.
- Prescribe step-by-step procedures only at known risk junctures.
- Leave internal implementation details to the worker within its permitted envelope.

## 4. Positive directives first, prohibitions second

Prioritize clear instructions on **what the agent must accomplish**. Reserve negative constraints (`FORBIDDEN`, `DO NOT`) for critical boundaries: protected branches, secrets, destructive migrations, and out-of-scope files.

## 5. Structural scaling

- Small tasks: Concise Markdown headings suffice.
- Complex tasks: Partition sections clearly: `OBJECTIVE`, `CONTEXT`, `BOUNDARIES`, `VERIFICATION`, and `OUTPUT CONTRACT`.
- Multi-layered data: Structured Markdown or XML-style tags may be used for readability, though specific formatting syntax remains an implementation choice rather than an invariant.

## 6. Governing rule

A prompt does not need to be long. A prompt must supply **sufficient signal so that an agent never guesses on decisions that alter system behavior**.