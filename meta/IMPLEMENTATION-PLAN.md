# AGENT-ORCHESTRATOR Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create a modular, model-agnostic, project-agnostic Markdown handbook that enables an AI Lead Architect to coordinate 5â€“10+ coding agents safely, with audit and sequential integration.

**Architecture:** The product consists of a canonical 7-chapter handbook and a separate operational toolkit: prompts, templates, checklists, playbooks, scenarios, case study, examples, and quick references. No executable runtime is implemented; every V2 primitive is converted into a protocol, decision procedure, or copy-paste template.

**Tech Stack:** Markdown, Git, Python standard library for validation/packaging only; ZIP + SHA-256 for the release artifact.

**Spec:** `docs/superpowers/specs/2026-09-23-agent-orchestrator-design.md`

## Global Constraints

- Content language: high-quality technical Vietnamese.
- Model-agnostic: no dependency on a specific provider.
- Project-agnostic: no assumption of Docker, npm, Go, Python, or a fixed toolchain.
- Strict on outcomes/invariants; adaptive on mechanisms.
- `WORKER_DONE != ACCEPTED` and evidence-before-acceptance are invariants.
- Do not create an orchestration runtime or executable control plane.
- No prohibited placeholder markers in the release.

## Review Focus

1. Does the framework accidentally make Git worktree/Docker mandatory instead of using them as adaptive defaults?
2. Does any prompt allow a worker to self-expand scope or self-assign `ACCEPTED`?
3. Does the audit verify expected files/rubric coverage, not only tests/diff?
4. Does integration follow the dependency DAG and revalidate after main/candidate drift?
5. Does recovery distinguish agent state from WP state and reject zombie output from an old generation?

---

### Task 1: Validation Contract and Skeleton

**Files:**
- Create: `tools/validate_framework.py`
- Create: the complete package directory skeleton

**Interfaces:**
- Produces: a validator that checks required files, headings, forbidden placeholders, and minimum prompt/scenario structure.
- Consumes: design spec acceptance criteria.

- [ ] **Step 1: Write the validator before content**: required file list and semantic checks.
- [ ] **Step 2: Run the validator and confirm FAIL** because the package does not yet exist.
- [ ] **Step 3: Create skeleton directories** without adding fake content stubs.
- [ ] **Step 4: Commit validator/skeleton.**

### Task 2: Canonical 7-Chapter Handbook

**Files:**
- Create: `AGENT-ORCHESTRATOR/handbook/01-core-philosophy.md`
- Create: `.../02-project-intake-dag-planning.md`
- Create: `.../03-resource-locking-boundaries.md`
- Create: `.../04-ready-to-use-prompts.md`
- Create: `.../05-zero-trust-forensic-audit.md`
- Create: `.../06-sequential-integration-cicd.md`
- Create: `.../07-adaptive-playbook.md`

**Interfaces:**
- Produces: canonical conceptual source for toolkit files.
- Consumes: V1/V2/finalization/anonymized field-experience synthesis.

- [ ] **Step 1:** Write 7 chapters with command examples and decision rules.
- [ ] **Step 2:** Run validator heading/coverage checks.
- [ ] **Step 3:** Review adaptive strictness and separation of duties.
- [ ] **Step 4:** Commit handbook.

### Task 3: Prompt Library and Operational Templates

**Files:**
- Create: six role prompts under `prompts/*.md`.
- Create: nine operational templates under `templates/*.md`.

**Interfaces:**
- Produces: copy-paste contracts for Lead, Worker, Auditor, Integrator, DR, and reporting.
- Consumes: handbook chapters 1â€“5.

- [ ] **Step 1:** Write the prompt library with Inputs/Hard Rules/Procedure/Output Contract/Stop Conditions.
- [ ] **Step 2:** Write templates for Intake, Profile, WP, Ownership, Resources, DR, Completion, Audit, Integration.
- [ ] **Step 3:** Run prompt/template completeness validation.
- [ ] **Step 4:** Commit prompts/templates.

### Task 4: Checklists, Playbooks, and 15 Scenarios

**Files:**
- Create: `checklists/*.md`
- Create: `playbooks/*.md`
- Create: `scenarios/01..15-*.md`

**Interfaces:**
- Produces: field procedures for execution, incidents, and edge cases.
- Consumes: handbook chapters 3,5,6,7.

- [ ] **Step 1:** Write pre-dispatch, completion, seven-step audit, pre-merge, and final-project checklists.
- [ ] **Step 2:** Write playbooks for scope, migration, Git conflict, crash, spec conflict, no-Docker, CI failure, Safe Mode, and drift/audit where needed.
- [ ] **Step 3:** Write all 15 scenarios using the required format.
- [ ] **Step 4:** Run scenario structural validation.
- [ ] **Step 5:** Commit operational procedures.

### Task 5: Case Study, Examples, and Quick Reference

**Files:**
- Create: `case-studies/project-neutral-six-agent-case.md`
- Create: `examples/*.md`
- Create: `quick-reference/*.md`

**Interfaces:**
- Produces: concrete runnable mental models and starter blueprints.
- Consumes: all framework primitives.

- [ ] **Step 1:** Write a project-neutral six-agent case study from anonymized/generalized incidents.
- [ ] **Step 2:** Write examples for six-agent, ten-agent, microservices, mobile, and CLI projects.
- [ ] **Step 3:** Write the Lead cheat sheet, state model, decision tree, and toolchain matrix.
- [ ] **Step 4:** Commit examples/reference.

### Task 6: Entry Points, Traceability, and Release QA

**Files:**
- Create: `README.md`, `START-HERE.md`, `CHANGELOG.md`, `LICENSE.md`
- Create: `meta/BUILD-SPEC.md`, `meta/IMPLEMENTATION-PLAN.md`, `meta/FINAL-VERIFICATION-REPORT.md`
- Create: `SHA256SUMS.txt`
- Create: release ZIP.

**Interfaces:**
- Produces: a navigable release package and evidence.
- Consumes: all prior tasks.

- [ ] **Step 1:** Write README/START-HERE with a 15-minute workflow.
- [ ] **Step 2:** Copy spec/plan into meta and create the source synthesis map.
- [ ] **Step 3:** Run the full validator, Markdown link checks, grep placeholder scan, and line-count/content checks.
- [ ] **Step 4:** Create SHA256SUMS, ZIP the package, re-extract the ZIP, and run the validator on the extracted artifact.
- [ ] **Step 5:** Write the Final Verification Report with exact command/results.
- [ ] **Step 6:** Commit release artifact metadata and tag candidate state in Git history (do not push/publish).
