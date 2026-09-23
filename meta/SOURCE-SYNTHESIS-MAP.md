# Source Synthesis Map

## V1: agent-orchestrator-framework

Retain: Zero Trust/Zero Hallucination, exhaustive rubric review, resource boundary enforcement, strict completion gate, forensic Git/filesystem/test verification, and anonymized field incidents.

## V2: orchestrator-framework-v2.0.0

Retain at the methodology level: Project Intelligence; Work Package contract; DAG; ownership modes; semantic resources; locks/leases; assignment generation; separate agent/WP states; Recovery Bundle; stale generation rejection; independent audit; immutable audit target; dependency-aware integration; Cross-WP Gate; 15 conformance scenarios.

Remove from the framework: any requirement for a Python runtime, persistent control plane, executable scheduler, schema validator service, or adapter runtime.

## AI Project Finalization Workflow V2

Retain: subject-specific source authority; state names as assurance levels; protected stop conditions; audit closure discipline; do not infer production validation from `BUILD_READY`.

## Previous Runtime

Extract the Orchestrator/Worker/Auditor/Integrator role prompts and evidence-before-acceptance rule. Do not reuse runtime code as the product core.

## Field Experience: anonymized six-agent project

Retain only generalized lessons: isolated workspaces; allocated semantic resource slots; infrastructure scope rejection; bounded shared-config extension; security-domain separation; missing-required-layer detection; external CI verification; sequential merge.

# Prompt Engineering Research Inputs: 2026-09-23

The following sources are used to **synthesize principles**, without copying prompts verbatim or making the framework vendor-dependent.

## addyosmani/agent-skills

Source: `https://github.com/addyosmani/agent-skills`

Retain:

- skills must describe executable workflows rather than provide reference prose only;
- verification, exit criteria, anti-patterns, and trigger conditions should be explicit;
- use progressive context instead of loading all knowledge into one prompt.

### Specific context-engineering skill

Source: `https://github.com/addyosmani/agent-skills/blob/main/skills/context-engineering/SKILL.md`

Retain:

- context hierarchy: persistent rules â†’ spec/architecture â†’ relevant source â†’ current errors/evidence â†’ history;
- load only the relevant spec fragment;
- read relevant files/tests/interfaces before editing;
- classify external/generated content as lower trust than project-owned sources;
- context quality > context quantity.

## github/awesome-copilot

Source: `https://github.com/github/awesome-copilot`

Retain:

- agent prompt anatomy: identity, responsibilities, methodology, constraints, output expectations;
- focused subagent wrappers with minimal shared context + exact expected outputs;
- imperative/action-oriented instructions;
- role-specific tool ceilings when supported by the harness;
- reusable skills/instructions/agents are different layers and should not be collapsed into one large prompt.

## anthropics/claude-plugins-official

Source: `https://github.com/anthropics/claude-plugins-official`

Retain:

- progressive disclosure: lean core + references/examples/scripts only when needed;
- skill/agent trigger descriptions must be specific;
- imperative instructions and focused domain boundaries;
- validate/test trigger quality and artifact quality before considering the artifact complete.

Vendor-specific frontmatter/tool syntax does not become an invariant.

## wshobson/agents

Source: `https://github.com/wshobson/agents`

Retain:

- one Markdown source can serve multiple harnesses when the body describes actions rather than tool vocabulary;
- a context file should act as a table of contents, with details in on-demand references;
- enforce invariants without hard-coding implementation;
- quality evaluation should combine a deterministic structural layer + semantic evaluation + repeated reliability checks when the cost is justified.

Model mappings and harness adapters in this repository are references only; do not hard-code them into the framework.

## muratcankoylan/Agent-Skills-for-Context-Engineering

Source: `https://github.com/muratcankoylan/agent-skills-for-context-engineering`

Retain:

- context includes the full inference state, not only prompt text;
- subagents primarily provide context isolation;
- a long-horizon brief requires definitions, an exact success predicate, non-counting outcomes, an adversarial failure-mode checklist, artifact-based reporting, and an audit-gated return;
- persistence must be paired with verification;
- early worker independence/diversity is useful in open-ended search;
- prompts should be lean and outcome-first; do not over-prescribe the path when invariant/outcome is already sufficiently clear.

## Prompt Synthesis Principle

The AGENT-ORCHESTRATOR Prompt Engineering System applies these principles:

```text
Compile project truth, do not decorate vague requests.
Curate context, do not dump context.
Lock outcomes and boundaries, not unnecessary implementation details.
Require evidence, not confidence.
Reject answer-shaped near misses explicitly when material.
Keep portable core independent from harness syntax.
```

# Overall Synthesis Principle

AGENT-ORCHESTRATOR preserves the **governance strength** of V1/V2 while using a **field manual + operational toolkit + Prompt Compiler methodology** delivery form. Strictness applies to invariants/evidence; mechanism and syntax adapt to the project/model/harness.

