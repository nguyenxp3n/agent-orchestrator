# AGENT-ORCHESTRATOR: Design Specification

**Ngày:** 2026-09-23  
**Trạng thái:** Được người dùng ủy quyền tự động duyệt toàn bộ các cổng thiết kế/thực thi  
**Loại sản phẩm:** Bộ cẩm nang Markdown + operational toolkit, không phải software runtime  
**Đối tượng:** AI Lead Architect / kỹ sư điều phối 5–10+ coding agents trên cùng dự án phần mềm

## 1. Mục tiêu

Xây dựng một framework phương pháp luận có thể dùng với Claude, GPT, Gemini, Antigravity, Cursor, Windsurf và các coding agents khác; có thể áp dụng cho Web Fullstack, Backend/Microservices, Mobile, Distributed Systems và CLI. Framework phải cho phép một AI Lead Architect đọc dự án, bóc tách Work Package, dựng DAG, cấp ownership và resource locks, tạo workspace cô lập, điều phối song song, xử lý Decision Request, audit độc lập và tích hợp tuần tự có bằng chứng.

## 2. Định vị

Agent Orchestrator không phải là orchestration engine Python, SDK, scheduler daemon hay control-plane executable. Agent Orchestrator là **field manual + operational toolkit** được AI Lead Architect thực thi. Các primitive như DAG, state machine, lock, recovery, audit attestation và integration queue được giữ ở mức giao thức, mẫu biểu, checklist và playbook; không triển khai thành runtime 400KB.

## 3. Nguồn dung hợp

1. V1 `agent-orchestrator-framework`: Zero Hallucination, Zero Trust, checklist exhaustive, resource boundary enforcement, strict completion gate, Forensic Audit và các field incidents đã được anonymize/generalize.
2. V2 `orchestrator-framework-v2.0.0`: Project Intelligence, Work Package protocol, DAG, Ownership Matrix, semantic resources, locks/leases, state machine, generation fencing, recovery bundle, 15 conformance scenarios, independent audit, dependency-aware integration, cross-WP gate.
3. `ai-project-finalization-workflow-v2.0.0`: source-of-truth theo subject, state names là assurance levels, stop conditions, audit closure discipline.
4. Runtime prototype cũ: prompt roles cho Orchestrator/Worker/Auditor/Integrator và các bài học về việc không biến `WORKER_COMPLETE` thành `ACCEPTED`.
5. Field experience 6-agent (anonymized): parallel branches, resource-slot allocation, scope rejection, missing required-layer detection, external CI verification và sequential merge.

## 4. Bốn invariant bất biến

### 4.1 Zero Hallucination
Không có bằng chứng thì trạng thái là `UNKNOWN`. Không bịa file, command output, CI status, commit, branch, resource allocation hay completion.

### 4.2 Zero Trust
Worker report là claim. Lead hoặc Auditor phải xác minh độc lập mọi trạng thái mutable trước khi dùng chúng làm truth.

### 4.3 Mandatory Verification
Mọi chuyển trạng thái quan trọng cần evidence thực tế: filesystem/Git diff/exit code/test log/CI/API hoặc artifact có thể kiểm chứng.

### 4.4 Atomic Completion
WP chỉ `ACCEPTED` khi 100% acceptance criteria bắt buộc đạt. Backend pass nhưng frontend thiếu vẫn là `INCOMPLETE`.

### 4.5 Project-Agnostic
Framework không được phụ thuộc một project/domain/case study cụ thể. Project input (`docs/`, `spec/`, `plan/`, `workflow/` hoặc nguồn tương đương) quyết định project model, ownership, resources, commands và verification. DB/migration/frontend/Docker chỉ xuất hiện khi project thật có chúng.

## 5. Adaptive Strictness

Framework cứng ở outcome/invariant, mềm ở mechanism. Isolation là bắt buộc nhưng có thể dùng Git worktree, clone riêng, container hoặc remote workspace. Quality gate là bắt buộc nhưng command phụ thuộc toolchain: `task qa`, `make test`, `pnpm test`, `cargo test`, `go test ./...`, `pytest`, `dotnet test`… Không ép Docker nếu dự án không dùng Docker.

## 6. Separation of Duties

- Lead Orchestrator: project intake, source-of-truth, decomposition, DAG, ownership, resource allocation, assignment, DR arbitration, recovery, acceptance, integration order.
- Worker: chỉ implementation trong WP và permission envelope.
- Auditor/Reviewer: độc lập kiểm chứng; không sửa code trong cùng audit task.
- Integrator: tích hợp candidate đã accepted; chỉ sửa hotspot theo Integration Request đã duyệt.

Lead không tự động viết code thay Worker. Nếu cần thay đổi implementation, phải giao lại Worker, tạo corrective WP hoặc thay Worker. Điều này giữ ownership, auditability và separation of duties.

## 7. Canonical Lifecycle

`PROJECT INPUT → INTAKE → PROJECT MODEL → WP DECOMPOSITION → DAG → OWNERSHIP/RESOURCE ALLOCATION → ISOLATED WORKSPACES → PARALLEL EXECUTION → WORKER_DONE → FORENSIC AUDIT → ACCEPTED/REWORK → INTEGRATION QUEUE → SEQUENTIAL MERGE → CROSS-WP/GLOBAL QA → MAIN/RELEASE`.

DAG quyết định integration order, không phải thời điểm worker báo xong.

## 8. State Model

WP states tối thiểu: `DRAFT`, `READY`, `ASSIGNED`, `RUNNING`, `BLOCKED`, `AWAITING_DECISION`, `READY_FOR_AUDIT`, `REWORK`, `ACCEPTED`, `INTEGRATING`, `INTEGRATED`, `FAILED`, `CANCELLED`.

Agent runtime state là khái niệm riêng: `READY`, `RUNNING`, `SUSPECTED_STALLED`, `PAUSED`, `FAILED`, `REASSIGNED`, `ABANDONED`. Worker crash không làm WP tự động thất bại.

## 9. Resource Model

Phải quản lý cả path và semantic resources. Ownership modes tham khảo: `EXCLUSIVE_WRITE`, `SHARED_READ`, `INTEGRATION_ONLY`, `ALLOCATED_WRITE`, `APPEND_ONLY`, `GENERATED`.

Semantic resources gồm: migration numbers, DB tables/columns, API routes, event names, queue names, env vars, ports, CLI commands, feature flags, schema versions, public interfaces.

Lead phân Migration/port slot trước khi dispatch; worker không tự cấp phát.

## 10. Work Package Contract

Mỗi WP tối thiểu có: ID, mục tiêu, source requirements, dependencies, allowed/readonly/forbidden paths, owned resources, expected files/artifacts, frozen contracts, acceptance criteria, commands, stop conditions, DR procedure, completion report contract, integration requests.

Worker không self-expand scope. Nhu cầu ngoài envelope phải trở thành Decision Request, Resource Request, Ownership Transfer, Integration Request hoặc WP mới.

## 11. Source-of-Truth

Không dùng một precedence ladder phẳng cho mọi thứ. Authority phải theo subject: product behavior, API wire shape, DB physical schema, security invariant, deployment, implementation order có thể thuộc các nguồn khác nhau. Nguồn ngang quyền mâu thuẫn thì giữ `UNRESOLVED`; không trộn thành giả thuyết.

## 12. Forensic Audit

Audit bắt buộc ít nhất bảy lớp: Git/identity, workspace hygiene, scope/diff, expected outputs, unit/target tests, repo-wide quality gate, acceptance barem + security/contracts/regression. Missing evidence = `UNKNOWN` và block acceptance. Audit bind với exact candidate SHA/snapshot; thay candidate sau audit thì audit cũ mất hiệu lực.

## 13. Sequential Integration

Chỉ candidate `ACCEPTED` mới vào integration queue. Merge theo dependency DAG, mỗi bước chạy pre-merge checks và post-merge global checks. Shared integration hotspots chỉ do Integrator xử lý. Main drift hoặc stale audited SHA làm batch phải revalidate.

## 14. Recovery

Khi agent crash: freeze workspace, capture status/diff/latest safe commit/leases/DRs/warnings, phân loại clean recoverable/dirty recoverable/corrupted/unsafe-unknown, tạo Recovery Bundle rồi tăng assignment generation. Zombie agent từ generation cũ bị reject.

## 15. Adaptive Playbook

Phải có playbook cho: no Docker/low RAM, agent timeout/crash, spec conflict, scope expansion, migration collision, Git conflict, CI failure, main drift, stale audit, cross-WP failure và Safe Mode.

## 16. 15 Scenario bắt buộc

1. Safe parallel work
2. Path ownership collision
3. Resource/migration collision
4. Agent crash/recovery
5. Zombie agent after reassignment
6. Contract change during downstream work
7. Integration hotspot conflict
8. Main drift
9. Harness/tool outage
10. Incomplete/contradictory docs
11. Stale audit SHA
12. Forbidden-path write
13. Independently passing WPs fail together
14. Duplicate/stale message
15. Corrupt orchestration state → Safe Mode

Mỗi scenario phải có Trigger, Risk, Evidence, Immediate Action, Forbidden Response, Recovery, Exit Criteria, Example Lead Response.

## 17. Package Structure

```text
AGENT-ORCHESTRATOR/
  README.md
  START-HERE.md
  handbook/01..07
  prompts/6 role prompts
  templates/9 operational templates
  checklists/5 checklists
  playbooks/8+ incident playbooks
  scenarios/15 scenario files
  case-studies/project-neutral-six-agent-case.md
  examples/5 project examples
  quick-reference/4 references
  meta/BUILD-SPEC.md
  meta/IMPLEMENTATION-PLAN.md
  meta/FINAL-VERIFICATION-REPORT.md
  SHA256SUMS.txt
```

## 18. Quality Requirements

- Tiếng Việt chuyên môn cao, rõ và trực tiếp.
- Mỗi khái niệm có ví dụ hoặc lệnh cụ thể khi phù hợp.
- Prompt phải copy-paste được, dùng placeholder rõ ràng.
- Template phải sử dụng được thủ công hoặc cho AI điền.
- Không có các placeholder markers bị cấm hoặc section rỗng.
- Không claim `production ready` hoặc `100%` nếu verification chưa đủ.
- README phải chỉ đường 15 phút để bắt đầu.
- Chương 7 và scenarios phải nhấn mạnh ứng biến theo toolchain/project thực tế.

## 19. Non-Goals

- Không xây Python/Node/Rust runtime.
- Không tạo database state store.
- Không viết scheduler executable.
- Không phụ thuộc một model/provider cụ thể.
- Không bắt buộc Docker/Kubernetes.
- Không hứa zero defects; chỉ enforce evidence-before-acceptance.

## 20. Acceptance Criteria

Artifact đạt yêu cầu khi:
1. Có đủ cấu trúc và file bắt buộc ở mục 17.
2. 7 handbook chapter bao phủ đầy đủ brief người dùng.
3. 6 prompt có role, constraints, inputs, output contract, stop/escalation rules.
4. Templates và checklists usable, không stub.
5. 15 scenarios đủ format chuẩn.
6. Project-neutral six-agent case study ghi lại ít nhất bốn bài học thực tế: missing required output layer, allocated resource slots, controlled scope expansion/security và external CI verification.
7. Có ví dụ 6-agent project-neutral và 10-agent project; ví dụ tổng thể phải chứng minh khả năng áp dụng cho Web, Microservices, Mobile, Distributed Systems và CLI.
8. Có validator chạy được để kiểm: file presence, forbidden placeholders, minimum sections, internal links cơ bản và checksum/archive integrity.
9. Final verification report ghi exact commands, exit codes và known limitations.