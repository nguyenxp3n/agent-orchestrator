# Source Synthesis Map

## V1: agent-orchestrator-framework

Retained: Zero Trust and Zero Hallucination invariants, exhaustive requirement barem reviews, strict filesystem resource boundary enforcement, atomic completion gates, multi-layered forensic Git, filesystem, and test verification, and anonymized multi-agent field incident logs.

## V2: orchestrator-framework-v2.0.0

Retained at methodology level: Project Intelligence ingestion, Work Package contracts, dependency DAGs, resource ownership modes, semantic resource allocation, leases and lock management, assignment generation counters, separate agent and Work Package state models, Recovery Bundles, stale generation rejection, independent forensic audits, immutable candidate commit binding, dependency-aware integration queues, Cross-WP Gates, and 15 standardized conformance scenarios.

Excluded from Agent Orchestrator: Mandating Python runtimes, persistent control plane daemons, executable scheduler binaries, schema validator web services, or specialized adapter runtimes.

## AI Project Finalization Workflow V2

Retained: Subject-based authority trees, status names reflecting genuine assurance levels, protected stop conditions, audit closure discipline, and explicitly rejecting the assumption that `BUILD_READY` implies production-verified readiness.

## Legacy Runtime Prototypes

Distilled into core role definitions for Orchestrator, Worker, Auditor, and Integrator alongside the foundational principle of evidence-before-acceptance. Runtime execution code was deliberately excluded to keep the framework portable.

## Multi-Agent Field Experience

Synthesized operational lessons: Fully isolated workspaces, reserved semantic resource slots, immediate out-of-scope boundary rejection, strictly bounded shared configuration extensions, distinct security domain boundaries, detection of missing required deliverable layers, independent external CI verification, and topological sequential merges.

# Prompt Engineering Research Inputs

The following external research sources were used to **synthesize underlying engineering principles**, avoiding verbatim prompt duplication or vendor-specific API dependencies.

## addyosmani/agent-skills

Source: `https://github.com/addyosmani/agent-skills`

Retained principles:
- Skills must define executable workflows rather than passive documentation.
- Verification procedures, exit criteria, known anti-patterns, and trigger conditions must be explicit.
- Context should be introduced progressively rather than loaded into a single monolithic prompt.

### Specific Context Engineering Insights

Source: `https://github.com/addyosmani/agent-skills/blob/main/skills/context-engineering/SKILL.md`

Retained principles:
- Context hierarchy: Persistent rules -> specification/architecture -> relevant source code -> current errors/evidence -> historical logs.
- Load only relevant specification fragments.
- Inspect related files, tests, and interfaces prior to making code modifications.
- Assign lower trust classifications to generated or external content compared to project-owned source code.
- Prioritize context quality and relevance over sheer volume.

## github/awesome-copilot

Source: `https://github.com/github/awesome-copilot`

Retained principles:
- Structured prompt anatomy: Identity, core responsibilities, execution methodology, constraints, and explicit output requirements.
- Focused subagent encapsulation with minimal shared context and exact expected deliverables.
- Imperative, action-oriented instructions.
- Role-specific tool ceilings where supported by the underlying harness.
- Maintain reusable skills, static instructions, and specialized agents as distinct architectural layers rather than collapsing them into a single massive prompt.

## anthropics/claude-plugins-official

Source: `https://github.com/anthropics/claude-plugins-official`

Retained principles:
- Progressive disclosure: Lean core instructions paired with on-demand references, examples, and scripts.
- Precise, unambiguous trigger definitions for skills and specialized agents.
- Imperative instructions combined with focused domain boundaries.
- Rigorous trigger and quality validation prior to considering an artifact complete.

Vendor-specific frontmatter syntax and tool-calling semantics are intentionally decoupled from the framework's core invariants.

## wshobson/agents

Source: `https://github.com/wshobson/agents`

Retained principles:
- A single Markdown specification can drive multiple agent harnesses by framing instructions around actions and outcomes rather than tool-specific vocabularies.
- Primary context files should function as structured tables of contents, delegating operational details to on-demand references.
- Enforce strict invariants while leaving non-critical implementation mechanics flexible.
- Multi-layered quality evaluation: Deterministic structural checks, semantic evaluation, and repeated reliability verifications where valuable.

Harness-specific model mappings and adapters in this repository serve as reference examples rather than rigid requirements.

## muratcankoylan/Agent-Skills-for-Context-Engineering

Source: `https://github.com/muratcankoylan/agent-skills-for-context-engineering`

Retained principles:
- Context encompasses the entire inference state, not merely the immediate prompt text.
- Subagents function primarily as context isolation boundaries.
- Long-horizon assignments require explicit definitions, exact success predicates, non-counting outcome verifications, adversarial failure-mode checklists, artifact-based deliverables, and audit-gated returns.
- Persistence must be paired with continuous independent verification.
- Early worker independence and divergence offer significant value during exploratory architectural phases.
- Prompts should remain lean and outcome-focused, avoiding over-prescription of implementation details once invariants and acceptance criteria are firmly established.

## Prompt Synthesis Principle

The Agent Orchestrator Prompt Engineering System operates on the following rules:

```text
Compile project truth: do not decorate vague user requests.
Curate targeted context: do not dump indiscriminate files.
Lock outcomes and boundaries: avoid dictating internal implementation choices.
Demand concrete evidence: disregard unsubstantiated agent claims.
Explicitly reject answer-shaped approximations on critical requirements.
Keep the core portable: avoid hard-coding harness-specific syntax.
```

# Overall Synthesis Summary

Agent Orchestrator preserves the **rigorous governance and quality controls** of V1 and V2 while delivering them as a **practical field manual, operational toolkit, and Prompt Compiler system**. Strictness focuses on invariants and verifiable evidence, while operational mechanics adapt smoothly across diverse projects, models, and execution harnesses.