# Checklist: Pre-Merge / Integration Gate

```text
[ ] WP status ACCEPTED
[ ] Audit disposition ACCEPT
[ ] Candidate SHA equals audited SHA
[ ] Dependencies already integrated or satisfied
[ ] Current main SHA compared with audit baseline
[ ] Main drift impact resolved
[ ] Migration/resource ordering valid
[ ] Integration Requests approved
[ ] Dedicated integration workspace clean
[ ] Merge policy confirmed
[ ] No ambiguous semantic conflict
```

Sau merge:

```text
[ ] Global build/test/quality gate exit 0 or documented non-regression baseline
[ ] Contract/schema/event validation pass
[ ] Critical E2E/smoke pass when required
[ ] Resulting integration SHA recorded
[ ] Cloud CI run belongs to resulting SHA when required
[ ] Queue stops immediately on blocking failure
```