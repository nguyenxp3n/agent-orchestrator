# Template: Decision Request

```text
DR_ID: DR-<number>
DATE: <timestamp>
AUTHOR_ROLE: <Lead | Worker | Auditor | Integrator>
AFFECTED_WPS: [<WP_ID>]

CONFLICT_OR_DECISION_DESCRIPTION:
<Clear statement of the ambiguity, specification conflict, or scope expansion request>

SUPPORTING_EVIDENCE:
- Source A: <file path, line reference, or commit>
- Source B: <competing file path or specification excerpt>

IMPACT_ANALYSIS:
- Architecture / contracts:
- Ownership / resources:
- Security / secrets:
- Concurrency / DAG schedule:

TECHNICAL_OPTIONS:
1. Option A: <description, advantages, risks>
2. Option B: <description, advantages, risks>

RECOMMENDED_ACTION:
<Preferred option with explicit rationale>

FINAL_RULING:
<Documented decision approved by human authority or Lead Arbitrator>

REQUIRED_ACTIONS:
- Contracts updated:
- Work Packages recompiled:
- Downstream audits invalidated:
```