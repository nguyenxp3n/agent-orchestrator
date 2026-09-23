# AGENT-ORCHESTRATOR Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Tạo một bộ cẩm nang Markdown module hóa, model-agnostic và project-agnostic để AI Lead Architect có thể điều phối 5–10+ coding agents an toàn, có audit và sequential integration.

**Architecture:** Sản phẩm gồm canonical handbook 7 chương và operational toolkit tách riêng: prompts, templates, checklists, playbooks, scenarios, case study, examples và quick references. Không triển khai runtime executable; mọi primitive V2 được chuyển thành giao thức, decision procedure và mẫu copy-paste.

**Tech Stack:** Markdown, Git, Python standard library chỉ để validation/packaging; ZIP + SHA-256 cho release artifact.

**Spec:** `docs/superpowers/specs/2026-09-23-AGENT-ORCHESTRATOR-design.md`

## Global Constraints

- Ngôn ngữ nội dung: tiếng Việt chuyên môn cao.
- Model-agnostic: không phụ thuộc provider cụ thể.
- Project-agnostic: không giả định Docker, npm, Go, Python hay một toolchain cố định.
- Strict on outcomes/invariants; adaptive on mechanisms.
- `WORKER_DONE != ACCEPTED` và evidence-before-acceptance là invariant.
- Không tạo orchestration runtime hoặc control plane executable.
- Không có các placeholder markers bị cấm trong release.

## Review Focus

1. Framework có vô tình biến Git worktree/Docker thành bắt buộc thay vì cơ chế mặc định thích ứng không?
2. Prompt có cho phép worker tự mở rộng scope hoặc tự nhận `ACCEPTED` không?
3. Audit có kiểm expected files/barem chứ không chỉ tests/diff không?
4. Integration có merge theo dependency DAG và revalidate khi main/candidate drift không?
5. Recovery có phân biệt agent state và WP state, đồng thời chặn zombie generation cũ không?

---

### Task 1: Validation Contract và Skeleton

**Files:**
- Create: `tools/validate_framework.py`
- Create: toàn bộ directory skeleton của package

**Interfaces:**
- Produces: validator kiểm required files, headings, forbidden placeholders, minimum prompt/scenario structure.
- Consumes: design spec acceptance criteria.

- [ ] **Step 1: Viết validator trước nội dung**: required file list và semantic checks.
- [ ] **Step 2: Chạy validator và xác nhận FAIL** vì package chưa tồn tại.
- [ ] **Step 3: Tạo skeleton directories** nhưng chưa thêm content stubs giả.
- [ ] **Step 4: Commit validator/skeleton.**

### Task 2: Canonical Handbook 7 Chương

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

- [ ] **Step 1:** Viết 7 chương với command examples và decision rules.
- [ ] **Step 2:** Chạy validator heading/coverage checks.
- [ ] **Step 3:** Rà soát adaptive strictness và separation of duties.
- [ ] **Step 4:** Commit handbook.

### Task 3: Prompt Library và Operational Templates

**Files:**
- Create: `prompts/*.md` sáu role prompts.
- Create: `templates/*.md` chín operational templates.

**Interfaces:**
- Produces: copy-paste contracts cho Lead, Worker, Auditor, Integrator, DR và reporting.
- Consumes: handbook chapters 1–5.

- [ ] **Step 1:** Viết prompt library với Inputs/Hard Rules/Procedure/Output Contract/Stop Conditions.
- [ ] **Step 2:** Viết templates cho Intake, Profile, WP, Ownership, Resources, DR, Completion, Audit, Integration.
- [ ] **Step 3:** Chạy validator prompt/template completeness.
- [ ] **Step 4:** Commit prompts/templates.

### Task 4: Checklists, Playbooks và 15 Scenarios

**Files:**
- Create: `checklists/*.md`
- Create: `playbooks/*.md`
- Create: `scenarios/01..15-*.md`

**Interfaces:**
- Produces: field procedures cho execution, incident và edge cases.
- Consumes: handbook chapters 3,5,6,7.

- [ ] **Step 1:** Viết pre-dispatch, completion, seven-step audit, pre-merge, final-project checklists.
- [ ] **Step 2:** Viết playbooks cho scope, migration, Git conflict, crash, spec conflict, no-Docker, CI failure, Safe Mode và drift/audit khi cần.
- [ ] **Step 3:** Viết đủ 15 scenario theo format chuẩn.
- [ ] **Step 4:** Chạy scenario structural validation.
- [ ] **Step 5:** Commit operational procedures.

### Task 5: Case Study, Examples và Quick Reference

**Files:**
- Create: `case-studies/project-neutral-six-agent-case.md`
- Create: `examples/*.md`
- Create: `quick-reference/*.md`

**Interfaces:**
- Produces: concrete runnable mental models và starter blueprints.
- Consumes: toàn bộ framework primitives.

- [ ] **Step 1:** Viết project-neutral six-agent case study từ các incident đã anonymize/generalize.
- [ ] **Step 2:** Viết ví dụ 6-agent, 10-agent, microservices, mobile, CLI.
- [ ] **Step 3:** Viết Lead cheat sheet, state model, decision tree, toolchain matrix.
- [ ] **Step 4:** Commit examples/reference.

### Task 6: Entry Points, Traceability và Release QA

**Files:**
- Create: `README.md`, `START-HERE.md`, `CHANGELOG.md`, `LICENSE.md`
- Create: `meta/BUILD-SPEC.md`, `meta/IMPLEMENTATION-PLAN.md`, `meta/FINAL-VERIFICATION-REPORT.md`
- Create: `SHA256SUMS.txt`
- Create: release ZIP.

**Interfaces:**
- Produces: navigable release package và evidence.
- Consumes: all prior tasks.

- [ ] **Step 1:** Viết README/START-HERE với 15-minute workflow.
- [ ] **Step 2:** Copy spec/plan vào meta và tạo source synthesis map.
- [ ] **Step 3:** Chạy full validator, markdown link checks, grep placeholder scan và line-count/content checks.
- [ ] **Step 4:** Tạo SHA256SUMS, ZIP, re-extract ZIP và chạy validator trên extracted artifact.
- [ ] **Step 5:** Viết Final Verification Report bằng exact command/result.
- [ ] **Step 6:** Commit release artifact metadata và tag candidate state trong git history (không push/publish).