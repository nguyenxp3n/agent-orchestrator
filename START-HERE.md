# START HERE: Chạy một phiên điều phối trong 15 phút

Tài liệu này là đường đi ngắn nhất để bắt đầu **bất kỳ software project nào**. Framework là Project-Agnostic: không yêu cầu project phải là Web, có Docker, có database hay dùng một toolchain cụ thể.

## 1. Intake project

Thu thập project input: tài liệu kiến trúc (`docs/` hoặc nguồn tương đương), đặc tả (`spec/` hoặc tương đương), kế hoạch (`plan/`), quy trình (`workflow/`), repository tree, build/test manifests, CI config, schema/migration nếu project có, env examples và project instructions. Các tên thư mục trên chỉ minh họa loại nguồn cần tìm; project không phải tuân theo convention này. Ghi lại facts, unknowns và contradictions bằng [Project Intake Template](templates/project-intake.md).

Không đoán command. Lead phải discover:

```bash
find . -maxdepth 2 -type f | sort
git status --short
ls package.json pyproject.toml go.mod Cargo.toml Taskfile.yml Makefile 2>/dev/null
find . -maxdepth 3 -type f | sort
```

## 2. Compile Project Execution Profile

Điền [Project Execution Profile](templates/project-execution-profile.md): architecture, toolchain, authoritative docs, commands, shared resources, integration hotspots, unknowns. Nếu hai tài liệu ngang quyền mâu thuẫn, trạng thái là `UNRESOLVED`, không tự hòa trộn.

## 3. Tách Work Packages

Mỗi WP phải là đơn vị có thể review độc lập. Dùng [Work Package Template](templates/work-package.md), bắt buộc có dependency, path scope, expected outputs, resources, tests, stop conditions và acceptance criteria.

## 4. Dựng DAG và Ownership Matrix

Ví dụ:

```text
WP-100 Contracts
   |
   +--> WP-210 Backend Auth ----+
   |                            +--> WP-500 Integration
   +--> WP-220 Frontend Auth ---+

WP-300 Infra can run parallel if it owns no shared DB/API hotspot.
```

Dùng [Ownership Matrix](templates/ownership-matrix.md) và [Resource Allocation](templates/resource-allocation.md). Phân migration slot/port trước khi dispatch.

## 5. Tạo workspace cô lập

Git worktree là mặc định tốt khi project dùng Git:

```bash
git worktree add ../wt-wp210 -b feat/wp-210
git worktree add ../wt-wp220 -b feat/wp-220
```

Nếu môi trường không hỗ trợ worktree, dùng clone riêng/container/remote workspace. Invariant là **không có hai active writers cùng ghi vào cùng working tree**.

## 6. Compile prompt và chạy Prompt Quality Gate

Điền [Prompt Compile Input](templates/prompt-compile-input.md), rồi dùng [Prompt Compiler](prompts/prompt-compiler.md) để compile prompt từ Project Truth + WP + Ownership + Resources + Acceptance. Chọn `Compact`, `Standard` hoặc `Long-Horizon` mode theo độ phức tạp.

Chạy [Prompt Quality Gate](prompt-engineering/07-prompt-quality-gate.md). Nếu disposition không phải `READY`, **không dispatch**.

## 7. Dispatch worker

Gửi [Worker Task Assignment Prompt](prompts/worker-task-assignment.md) đã được compile với exact objective, success predicate, context references, boundaries, resources, non-counting outcomes, verification và output contract. Lead giữ ledger về branch/worktree, generation, resources và dependency status.

## 8. Khi worker hỏi

- Câu hỏi implementation trong scope → dùng [Clarification Guidance](prompts/clarification-guidance.md).
- Spec/reality conflict, security decision, scope expansion → dùng [Architectural Arbitration](prompts/architectural-arbitration.md) + [Decision Request](templates/decision-request.md).
- Worker xin file/resource thuộc WP khác → mặc định từ chối, rồi tìm phương án trong phạm vi hoặc tạo request chính thức.

## 9. Worker báo xong ≠ xong

Worker phải gửi [Completion Report](templates/completion-report.md). Sau đó một auditor/Lead độc lập chạy [Seven-Step Forensic Audit](checklists/seven-step-forensic-audit.md).

```bash
git status --short
git log -1 --stat
git diff <base>...HEAD --name-status
# chạy project-specific unit / quality commands từ Project Execution Profile
```

Nếu thiếu frontend/migration/docs/test theo barem, disposition là `REJECT/REWORK` dù test của phần đã làm đang xanh.

## 10. Integrate tuần tự

Candidate `ACCEPTED` mới vào queue. Merge theo DAG, không theo “ai xong trước”. Với policy cho phép merge commit:

```bash
git merge --no-ff feat/wp-210
# run global QA
git merge --no-ff feat/wp-220
# run global QA again
```

Nếu main thay đổi sau audit, rebase/recreate candidate và re-audit khi identity thay đổi.

## 11. Kết thúc

Dùng [Final Project Gate](checklists/final-project-gate.md). Chỉ nói `FINAL` khi mọi critical criterion có evidence; nếu chưa thể chạy cloud/deployment, ghi rõ assurance level hiện tại thay vì nâng trạng thái bằng ngôn từ.