# 05: Verification, Non-Counting Outcomes & Return Gates

## Verification is part of the prompt, not optional post-processing

Every agent-grade prompt must state what evidence proves the result.

```text
Claim -> Evidence -> Identity -> Criterion
```

Example:

```text
Claim: tests pass
Evidence: command + exit code + relevant summary
Identity: candidate SHA / workspace generation
Criterion: AC-03
```

## Success Predicate

Write one observable completion condition. Avoid terms such as “high quality,” “complete,” or “stable” without an operational definition.

## Non-Counting Outcomes

**Non-Counting Outcomes** are artifacts that resemble completion but do not satisfy intent.

Software WP examples:

- implement only one layer when acceptance requires multiple layers;
- add tests without implementing behavior;
- obtain a green build by removing/skipping tests;
- rely on an unverified assumption;
- return a design/proposal instead of a code artifact;
- silently narrow scope;
- run verification on a SHA different from the candidate;
- external CI is green but belongs to a different commit.

## Failure-Mode Checklist for the Auditor

A generic instruction such as “review carefully” is weak. For a high-risk task, compile a domain-specific failure-mode list for the Auditor to actively search.

```text
AUDIT_FAILURE_MODES:
- duplicate side effect under retry
- migration ordering collision
- backward-incompatible contract change
- untracked generated artifact
- secret/log leakage
```

## Return Gate

The return condition must be a predicate over artifact/evidence:

```text
Return WORKER_COMPLETE_CLAIM only when all required expected outputs exist and all worker-owned verification steps have current-session evidence.
```

Do not use confidence as a gate:

```text
"Report done when you feel confident"  # invalid
```

## Persistence Rule

Use persistence on long-running tasks only when paired with matching verification. “Do not stop until done” without a success predicate encourages reward-hacking/answer-shaped near misses.

## Fresh-context Verification

When possible, the Auditor should use fresh context and a clear candidate identity. The artifact author can rationalize its own gaps; independent audit reduces that risk.

## Evidence freshness

Evidence becomes stale when:

- candidate changes after audit;
- base/main drift changes assumptions;
- generated artifact is regenerated;
- external dependency/contract changes;
- command runs in a different workspace.

Stale evidence does not automatically carry forward.
