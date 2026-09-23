# Changelog

## Prompt Engineering Upgrade: 2026-09-23

- Introduced the Prompt Engineering System across seven modules: anatomy, compiler protocol, context engineering, boundary design, verification and return gates, model-agnostic portability, and the Prompt Quality Gate.
- Added `prompts/prompt-compiler.md`, Prompt Compile Input template, Prompt Quality Report template, and the compiler cheat sheet.
- Upgraded six canonical role prompts with unified fields: objective, success predicate, context references, ownership, resources, non-counting outcomes, evidence, stop conditions, and return semantics.
- Added three execution modes: Compact, Standard, and Long-Horizon.
- Integrated progressive disclosure, context trust tiers, and just-in-time context selection.
- Added adversarial red-team review for sensitive prompts and non-counting outcome checks to eliminate answer-shaped near misses.
- Migrated canonical prompt instructions to action-outcome phrasing for portability across Claude, OpenAI Codex, Copilot, Cursor, Gemini, and custom harnesses.
- Synthesized industry principles from open-source agent patterns and context engineering frameworks.

## Release: 2026-09-23

- Project-agnostic hardening: removed project-specific identities and case-bound names; generalized multi-agent case studies, domain labels, resource slots, shared-configuration examples, and intake semantics.
- Structured framework as an operational field manual and governance toolkit rather than an executable Python runtime.
- Unified core principles: Zero Hallucination, Zero Trust, Mandatory Verification, Atomic Completion.
- Added Adaptive Strictness to enforce verification quality without forcing specific container runtimes or toolchains.
- Modularized prompt libraries, templates, checklists, playbooks, fifteen risk scenarios, examples, and quick references.
- Maintained core invariants: `WORKER_DONE != ACCEPTED`, immutable audit identity, generation fencing, and sequential dependency-aware integration.