# AGENT-ORCHESTRATOR

**Model-Agnostic · Project-Agnostic · Adaptive · Evidence-Driven**

Agent Orchestrator là bộ cẩm nang vận hành cho AI Lead Architect điều phối 5–10+ coding agents trên cùng một software project. Package tập trung vào phương pháp luận, prompts, templates, checklists, playbooks và case studies; nó không triển khai orchestration engine, SDK hay scheduler runtime. Lead có thể áp dụng bộ tài liệu này trên Claude, GPT/Codex, Gemini/Antigravity, Cursor, Windsurf hoặc harness tương đương.

> Invariant trung tâm: `WORKER_DONE != ACCEPTED`. Worker chỉ báo cáo claim; acceptance chỉ xuất hiện sau independent verification có evidence.

## Project-Agnostic: dùng cho mọi dự án phần mềm

Agent Orchestrator áp dụng cho nhiều loại software project: Web Fullstack, Microservices Backend, Mobile App, Distributed Systems, CLI/Developer Tools và các cấu trúc khác. Lead cần đủ project input để xây Project Model và Project Execution Profile trước khi điều phối.

Project input thường gồm `docs/`, `spec/`, `plan/`, `workflow/`, repository tree, build/test manifests, CI config, DB/schema/env information và project instructions. Đây là **các loại thông tin logic, không phải tên thư mục bắt buộc**: nếu project dùng ADR/RFC/tickets, Makefile, Gradle, Cargo, Xcode, Bazel, Terraform hay nguồn tương đương thì Lead phải discover và dùng nguồn thực tế đó.

Nguyên tắc: **project truth quyết định framework adaptation; framework không ép project phải giống một case study mẫu.**

## Framework xử lý vấn đề gì?

Khi nhiều AI cùng sửa một repository, rủi ro lớn nhất tập trung ở coordination giữa các agent, không phải khả năng viết code riêng lẻ: hai agent cùng sửa migration; một worker lấn sang file của worker khác; agent báo hoàn tất nhưng bỏ frontend; một branch pass độc lập nhưng fail khi ghép; CI cloud khác local; worker crash rồi quay lại gửi kết quả cũ; main drift sau audit. Agent Orchestrator biến các rủi ro đó thành một quy trình có thể kiểm soát.

## Bốn nguyên tắc bất biến

1. Zero Hallucination: không có evidence thì ghi `UNKNOWN`.
2. Zero Trust: báo cáo agent là claim, không phải truth.
3. Mandatory Verification: mọi transition quan trọng phải có bằng chứng thực tế.
4. Atomic Completion: thiếu một acceptance criterion bắt buộc thì WP chưa hoàn thành.

Framework áp dụng nguyên tắc **Strict on outcomes, adaptive on mechanisms**. Isolation là bắt buộc; cơ chế có thể là Git worktree, clone riêng, container hoặc remote workspace. Quality gate cũng bắt buộc, nhưng Lead phải discover command từ project thay vì ép `task qa` hay Docker vào mọi repository.

## Lifecycle chuẩn

```text
PROJECT INPUT
  -> INTAKE
  -> PROJECT MODEL
  -> WORK PACKAGE DECOMPOSITION
  -> DEPENDENCY DAG
  -> OWNERSHIP + RESOURCE ALLOCATION
  -> ISOLATED WORKSPACES
  -> SAFE PARALLEL EXECUTION
  -> WORKER_DONE
  -> ZERO-TRUST FORENSIC AUDIT
  -> ACCEPTED / REWORK
  -> INTEGRATION QUEUE
  -> SEQUENTIAL MERGE
  -> CROSS-WP + GLOBAL QA
  -> MAIN / RELEASE
```

## Đọc theo mục đích

- Bắt đầu trong 15 phút: [START-HERE.md](START-HERE.md).
- Hiểu triết lý và role boundaries: [handbook/01-core-philosophy.md](handbook/01-core-philosophy.md).
- Intake, WP, DAG, worktrees: [handbook/02-project-intake-dag-planning.md](handbook/02-project-intake-dag-planning.md).
- Paths, locks, migration slots: [handbook/03-resource-locking-boundaries.md](handbook/03-resource-locking-boundaries.md).
- Prompt Engineering + prompt compiler + role prompts: [handbook/04-ready-to-use-prompts.md](handbook/04-ready-to-use-prompts.md), thư mục `prompt-engineering/` và `prompts/`.
- Audit độc lập: [handbook/05-zero-trust-forensic-audit.md](handbook/05-zero-trust-forensic-audit.md).
- Sequential integration + CI/CD: [handbook/06-sequential-integration-cicd.md](handbook/06-sequential-integration-cicd.md).
- Edge cases và ứng biến: [handbook/07-adaptive-playbook.md](handbook/07-adaptive-playbook.md).
- Project-type examples (non-prescriptive): [examples/README.md](examples/README.md).


## Prompt Compiler: biến project truth thành prompt sẵn sàng dispatch

Agent Orchestrator không yêu cầu Lead tự “viết prompt hay” cho từng agent. Lead compile prompt từ Work Package, source authority, ownership, resource allocation, expected outputs và verification contract. Bắt đầu tại `prompt-engineering/01-prompt-anatomy.md`, dùng `prompts/prompt-compiler.md`, rồi chạy Prompt Quality Gate trước dispatch.

Ba mode: **Compact** cho task nhỏ, **Standard** cho WP thông thường, **Long-Horizon** cho task dài/rủi ro cần success predicate, non-counting outcomes, adversarial verification và audit-gated return.

## Quy tắc vận hành

Lead Orchestrator chịu trách nhiệm decomposition, architecture constraints, ownership, resources, decision arbitration, audit disposition và integration order. Worker triển khai trong phạm vi WP; Auditor xác minh độc lập; Integrator xử lý merge đã phê duyệt. Separation of duties ngăn Lead âm thầm sửa code rồi tự nghiệm thu chính thay đổi đó.

## Giới hạn bảo đảm

Framework không hứa zero defects hoặc loại bỏ hallucination tuyệt đối. Mục tiêu enforceable là: **không chấp nhận completion claim chưa được kiểm chứng, không cố ý cho phép write ngoài scope, và không merge candidate đã mất tính đồng nhất với evidence audit.**