# AGENT-ORCHESTRATOR Implementation Plan

> **For agentic workers:** Use subagent-driven development or plan execution skills to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create a modular, model-agnostic, and project-agnostic Markdown field manual and operational toolkit enabling an AI Lead Architect to coordinate 5 to 10+ coding agents safely through independent audits and sequential integration.

**Architecture:** The deliverable consists of a canonical 7-chapter handbook alongside specialized operational toolkits: role prompts, templates, checklists, incident playbooks, scenarios, case studies, examples, and quick-reference guides. It avoids executable runtime daemons; all operational primitives exist as protocols, decision trees, and copy-paste templates.

**Tech Stack:** Markdown, Git, Python standard library strictly for validation and packaging scripts; ZIP and SHA-256 for release artifacts.

**Spec:** `meta/BUILD-SPEC.md`

## Global Constraints

- Content language: Professional, human-written technical English.
- Model-agnostic: Zero proprietary lock-in to specific model providers.
- Project-agnostic: No mandatory assumptions regarding Docker, npm, Go, or Python.
- Strict on outcomes and invariants; adaptable on operational mechanisms.
- `WORKER_DONE != ACCEPTED` and evidence-before-acceptance remain non-negotiable invariants.
- No orchestration runtime or control-plane executable.
- Zero prohibited placeholder markers in release documents.

## Review Focus

1. Does the framework avoid making Git worktrees or Docker mandatory instead of an adaptable default mechanism?
2. Do prompts prevent workers from autonomously expanding scope or claiming `ACCEPTED` status?
3. Do audits inspect deliverables and rubrics rather than checking only raw test outputs or diffs?
4. Does integration sequence branches according to dependency DAGs and trigger revalidation upon branch drift?
5. Does failure recovery cleanly decouple agent state from Work Package state and block stale generation messages?

---

### Task 1: Validation Contract and Skeleton

**Files:**
- Create: `tools/validate_framework.py`
- Create: Complete directory skeleton for the framework package

**Interfaces:**
- Produces: Automated validator checking required files, headings, prohibited placeholders, and minimum prompt/scenario structures.
- Consumes: Design specification acceptance criteria.

- [x] **Step 1: Write validator prior to content implementation**: enforce required file lists and semantic checks.
- [x] **Step 2: Run validator and confirm failure** on an empty package.
- [x] **Step 3: Create skeleton directories** without placeholder stubs.
- [x] **Step 4: Commit validator and directory skeleton.**

### Task 2: Canonical Handbook (7 Chapters)

**Files:**
- Create: `handbook/01-core-philosophy.md`
- Create: `handbook/02-project-intake-dag-planning.md`
- Create: `handbook/03-resource-locking-boundaries.md`
- Create: `handbook/04-ready-to-use-prompts.md`
- Create: `handbook/05-zero-trust-forensic-audit.md`
- Create: `handbook/06-sequential-integration-cicd.md`
- Create: `handbook/07-adaptive-playbook.md`

**Interfaces:**
- Produces: Canonical conceptual foundations for operational toolkit files.
- Consumes: Synthesized insights from V1, V2, and anonymized multi-agent field experience.

- [x] **Step 1:** Write the 7 chapters with shell command examples and clear decision trees.
- [x] **Step 2:** Execute validator heading and coverage checks.
- [x] **Step 3:** Review adaptive strictness and separation of duties.
- [x] **Step 4:** Commit canonical handbook.

### Task 3: Prompt Library and Operational Templates

**Files:**
- Create: `prompts/*.md` canonical role prompts.
- Create: `prompt-engineering/*.md` prompt compiler modules.
- Create: `templates/*.md` operational templates.

**Interfaces:**
- Produces: Production-ready contracts for Lead, Worker, Auditor, Integrator, Decision Requests, and progress reporting.
- Consumes: Handbook chapters 1 through 5.

- [x] **Step 1:** Write prompt library with explicit inputs, hard rules, execution flows, output contracts, and stop conditions.
- [x] **Step 2:** Write templates for Intake, Profile, Work Packages, Ownership, Resources, Decision Requests, Completion, Audits, and Integration.
- [x] **Step 3:** Validate prompt and template structural completeness.
- [x] **Step 4:** Commit prompts and templates.

### Task 4: Checklists, Playbooks, and 15 Conformance Scenarios

**Files:**
- Create: `checklists/*.md`
- Create: `playbooks/*.md`
- Create: `scenarios/01..15-*.md`

**Interfaces:**
- Produces: Field operational procedures for execution, incidents, and edge cases.
- Consumes: Handbook chapters 3, 5, 6, and 7.

- [x] **Step 1:** Write pre-dispatch, worker completion, seven-step forensic audit, pre-merge, and final project checklists.
- [x] **Step 2:** Write playbooks for scope expansion, migration collisions, git conflicts, crashes, spec conflicts, environments without Docker, CI failures, Safe Mode, and branch drift.
- [x] **Step 3:** Write all 15 operational scenarios using the standardized structure.
- [x] **Step 4:** Execute scenario structural validation.
- [x] **Step 5:** Commit checklists, playbooks, and scenarios.

### Task 5: Case Studies, Examples, and Quick Reference

**Files:**
- Create: `case-studies/project-neutral-six-agent-case.md`
- Create: `examples/*.md`
- Create: `quick-reference/*.md`

**Interfaces:**
- Produces: Concrete reference models and quickstart implementation blueprints.
- Consumes: All core framework primitives.

- [x] **Step 1:** Write project-neutral six-agent case study based on anonymized field incidents.
- [x] **Step 2:** Write architecture examples for 6-agent, 10-agent, microservices, mobile, and CLI systems.
- [x] **Step 3:** Write Lead cheat sheet, state model guide, decision trees, and toolchain adaptation matrices.
- [x] **Step 4:** Commit case studies, examples, and quick references.

### Task 6: Entry Points, Traceability, and Release QA

**Files:**
- Create: `README.md`, `START-HERE.md`, `CHANGELOG.md`, `LICENSE.md`
- Create: `meta/BUILD-SPEC.md`, `meta/IMPLEMENTATION-PLAN.md`, `meta/FINAL-VERIFICATION-REPORT.md`, `meta/SOURCE-SYNTHESIS-MAP.md`
- Create: `SHA256SUMS.txt`
- Create: Release ZIP package.

**Interfaces:**
- Produces: Navigable release package backed by verification evidence.
- Consumes: All prior task deliverables.

- [x] **Step 1:** Write `README.md` and `START-HERE.md` featuring a 15-minute quickstart workflow.
- [x] **Step 2:** Maintain design specifications, plans, and source synthesis maps in `meta/`.
- [x] **Step 3:** Run full validation suite, markdown link checks, and placeholder scans.
- [x] **Step 4:** Generate `SHA256SUMS.txt`, build ZIP package, extract into clean directory, and re-run validation against extracted files.
- [x] **Step 5:** Generate Final Verification Report documenting exact commands and results.
- [x] **Step 6:** Localize all documentation and metadata to professional, human-written technical English.