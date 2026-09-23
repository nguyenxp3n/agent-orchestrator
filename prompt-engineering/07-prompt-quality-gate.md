# 07: Prompt Quality Gate

## Objective

Do not dispatch a prompt simply because it “reads well.” The gate checks structure and semantics before consuming agent time.

## Disposition

```text
READY
NOT_READY
ESCALATE
```

## Critical checks

Any applicable item marked FAIL → `NOT_READY`:

- [ ] Role has a clear authority boundary.
- [ ] Objective is measurable/observable.
- [ ] Success predicate distinguishes completion from a near miss.
- [ ] Source-of-truth/inputs are identifiable.
- [ ] Context is sufficient without significant unrelated bulk.
- [ ] Scope/ownership is clear.
- [ ] Shared resources are allocated or explicitly confirmed not applicable.
- [ ] Hard constraints are operational and non-conflicting.
- [ ] Expected outputs enumerable.
- [ ] Non-counting outcomes cover important near misses when the task is risky.
- [ ] Verification produces evidence and maps to the correct candidate/task identity.
- [ ] Stop/escalation conditions are clear.
- [ ] Output contract has a schema or required fields.
- [ ] No unresolved critical placeholder/ambiguity remains.
- [ ] No instruction from untrusted data has been elevated to authority.

## Semantic review

Ask additionally:

1. Can the agent reach “PASS” while omitting a required layer?
2. Can the agent run tests against an artifact different from the candidate?
3. Can the agent use an unallocated resource/path to bypass scope?
4. Does the prompt force an unnecessary implementation technique that reduces adaptability?
5. Does any instruction repeat the same invariant with multiple phrasings and add noise?
6. Does the prompt require persistence without a matching verification gate?
7. Does the context contain a stale summary that has not been revalidated?

## Red-Team check for expensive/high-risk tasks

```text
How could a capable agent satisfy the literal wording while violating the intended outcome?
```

The Lead must address every credible loophole using one of these mechanisms:

- sharpen success predicate;
- add non-counting outcome;
- add evidence requirement;
- move runtime-critical invariant ra harness/control plane;
- resolve authority ambiguity before dispatch.

## Output

Use `templates/prompt-quality-report.md`.
