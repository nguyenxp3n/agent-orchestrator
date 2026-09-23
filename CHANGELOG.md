# Changelog

## Prompt Engineering Upgrade: 2026-09-23

- Added a **Prompt Engineering System** with 7 modules: anatomy, compiler protocol, context engineering, boundary design, verification/return gates, model-agnostic portability, and Prompt Quality Gate.
- Added `prompts/prompt-compiler.md`, Prompt Compile Input, Prompt Quality Report, and the compiler cheat sheet.
- Upgraded 6 canonical role prompts to share objective/success predicate/context references/ownership/resources/non-counting outcomes/evidence/stop/output semantics.
- Added three modes: Compact, Standard, Long-Horizon.
- Integrated progressive disclosure, context trust classes, and just-in-time context selection.
- Added an adversarial red-team step for expensive/high-risk prompts and non-counting outcomes to prevent answer-shaped near misses.
- Converted canonical prompt content to action/outcome wording to improve portability across Claude, Codex, Copilot, Cursor, Gemini, and equivalent harnesses.
- Added source synthesis from addyosmani/agent-skills, github/awesome-copilot, anthropics/claude-plugins-official, wshobson/agents, and Agent-Skills-for-Context-Engineering.

## Initial Release: 2026-09-23

- Project-Agnostic hardening: removed project identity/case-specific naming from the artifact; generalized the six-agent case, domain labels, resource slots, shared-config examples, and intake semantics.
- Repositioned the framework as a field manual + operational toolkit rather than a Python orchestration runtime.
- Synthesized V1 discipline, V2 DAG/state/resource/recovery, finalization assurance levels, and anonymized field incidents.
- Standardized four invariants: Zero Hallucination, Zero Trust, Mandatory Verification, Atomic Completion.
- Added Adaptive Strictness to preserve quality without forcing Docker or a specific toolchain.
- Split out the prompt library, templates, checklists, playbooks, 15 scenarios, examples, and quick references.
- Preserved `WORKER_DONE != ACCEPTED`, immutable audit identity, generation fencing, and sequential dependency-aware integration.

