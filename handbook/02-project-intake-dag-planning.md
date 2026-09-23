# Chương 2: Tiếp nhận dự án và lập kế hoạch tổng thể

## 2.0 Project-Agnostic Intake Rule

Không bắt đầu decomposition bằng cách copy cấu trúc từ project khác. Lead phải xác định project type, authoritative inputs, toolchain, module boundaries, verification commands và semantic resources từ repository đang nhận. `docs/`, `spec/`, `plan/`, `workflow/` là các category đầu vào; file/folder thật có thể mang tên khác.

Nếu project không có DB thì bỏ migration allocation. Nếu không có frontend thì không tạo frontend WP. Nếu là mobile, CLI hoặc distributed system thì ownership matrix phải phản ánh screens/modules/commands/protocols/resources thực tế.

## 2.1 Mục tiêu của Project Intake

Không dispatch agent khi Lead chưa hiểu repository đủ để biết source of truth nằm đâu, project build/test thế nào, module boundaries ra sao, resource nào là shared hotspot và unknown nào có thể làm vỡ planning. Intake không phải đọc mọi dòng code; nó là quá trình tạo **Project Execution Profile** dựa trên evidence.

## 2.2 Bước 1: Phân tích ngữ cảnh

Ưu tiên đọc:

1. Project instructions: `AGENTS.md`, `CLAUDE.md`, `GEMINI.md`, contribution docs.
2. `docs/`, `spec/`, `plans/`, ADRs, API schemas.
3. Repository tree và module manifests.
4. Toolchain: package/build files, task runners, lockfiles.
5. DB/migrations/env samples.
6. Tests và CI/CD.
7. Git history gần đây khi cần hiểu conventions hoặc in-flight migrations.

Evidence discovery mẫu:

```bash
pwd
git status --short
git log -10 --oneline
find . -maxdepth 2 -type f | sort
find . -maxdepth 4 -type f | sort
```

Nếu project lớn, inventory theo module thay vì dump mọi file vào context.

## 2.3 Từ facts sang Project Execution Profile

Profile phải trả lời: kiến trúc/module chính; commands canonical; source authority theo subject; shared integration hotspots; semantic resources; generated/read-only files; frozen contracts; baseline failures; unknowns block planning.

Không serialize secrets để “chứng minh” `.env` tồn tại. Chỉ record path và policy.

## 2.4 Bước 2: Bóc tách module thành Work Package

WP là đơn vị nhỏ nhất có thể **giao, audit và accept độc lập**. Một WP tốt có outcome rõ, boundary rõ và acceptance rõ.

```text
Bad:  WP: Implement the whole platform
Good: WP-A Domain A implementation + assigned resources + tests
      WP-B Client/consumer integration + tests (only if required)
      WP-C Freeze shared contract used by A/B
```

Không tách quá nhỏ đến mức mọi worker buộc phải cùng sửa một file trung tâm. Decomposition tối ưu **safe parallelism**, không tối đa số agent.

## 2.5 Ownership Matrix

Trước parallel wave, lập Code Ownership Matrix:

| WP | allowed_paths | readonly_paths | integration-only | semantic resources |
|---|---|---|---|---|
| WP-A | `<domain-a-path>/**`, allocated resource R1 | `<frozen-contract>` | `<integration-hotspot>` | `<domain-a-resource>` |
| WP-B | `<client-or-consumer-path>/**` | `<frozen-contract>` | `<global-registration-hotspot>` | contract consumer |
| WP-C | `<infra-or-platform-path>/**` | service/runtime manifests | `<shared-schema-or-domain-hotspot>` | assigned ports/resources |

Ownership không chỉ là paths. Hai workers có thể sửa file khác nhau nhưng collision trên cùng API route hoặc env var.

## 2.6 Bước 3: Xây Dependency DAG

Phân dependencies:

- Hard dependency: downstream chưa thể bắt đầu.
- Contract dependency: có thể chạy khi contract đã freeze.
- Soft dependency: hữu ích nhưng không block.
- Integration dependency: ảnh hưởng merge order/cross-WP gate.

```text
WP-0 Freeze shared contract(s)
  |------> WP-A Domain implementation
  |------> WP-B Consumer/client implementation
  |------> WP-C Platform/integration dependency

WP-A + WP-B + WP-C
  |
  v
WP-Z Cross-WP Integration/E2E
```

Nếu shared contract chưa freeze mà producer/consumer workers cùng tự đoán interface, đó là deferred conflict chứ không phải parallelism.

## 2.7 Chọn parallel waves

Một WP chỉ vào cùng wave khi:

```text
all hard deps satisfied
AND no overlapping exclusive path ownership
AND no exclusive resource collision
AND required contracts frozen
AND workspace available
AND verification commands known enough
```

Ví dụ 6-agent project-neutral:

```text
Wave 0: Contract/architecture freeze
Wave 1 parallel:
  A1 Domain A                Resource Slot R1
  A2 Domain B                Resource Slot R2
  A3 Domain C                Resource Slot R3
  A4 Client/Consumer         Resource Slot R4 or contract-consumer only
  A5 Infrastructure         isolated infra ownership
  A6 CI/CD                  workflow-only ownership
Wave 2: sequential integration by DAG + global QA
```

A5 phát hiện cần một shared resource ngoài allocation không tự tạo/claim; phải gửi request. Với DB project resource đó có thể là migration slot, nhưng framework không giả định DB.

## 2.8 Bước 4: Cấp phát Git Worktree / Workspace

Với Git:

```bash
git worktree add ../wt-wp210 -b feat/wp-210
git worktree add ../wt-wp220 -b feat/wp-220
git worktree list
```

Mỗi coding worker có working tree riêng. Không dùng một working tree rồi chỉ bảo agents “đừng đụng nhau”. Isolation phải vật lý hoặc được harness enforce.

Nếu không thể dùng worktree: separate clone; per-agent remote workspace; per-agent container/VM; docs-only có thể dùng isolated directory.

## 2.9 Baseline và branch identity

Trước dispatch ghi:

```text
wp_id
branch
worktree_path
base_sha
assignment_generation
allowed/readonly/forbidden
resource leases
verification commands
```

Base SHA cần cho audit diff và main drift detection.

## 2.10 Expected Files là một phần của planning

Đừng đợi audit mới nghĩ file nào phải tồn tại. WP planning phải liệt kê expected outputs theo tầng:

```text
Project-specific persistence/schema: required artifact(s) if the project has them
Domain implementation: modules/services/types/tests
Client/consumer surface: UI, SDK, CLI registration, mobile screen, adapter or equivalent if required
Contracts: frozen API/proto/schema/interface as applicable
Docs: update only if WP requires
```

Expected files giúp bắt “half-complete” mà tests không bắt được.

## 2.11 Planning khi docs không hoàn chỉnh

Adaptive procedure:

1. Inventory facts từ code/manifests/CI.
2. Tách `FACT`, `INFERENCE`, `UNKNOWN`.
3. Chỉ dùng inference low-risk để planning; protected ambiguity thành DR.
4. Không invent architecture vì docs thiếu.
5. Thu hẹp WP nếu boundary chưa đủ chắc.

## 2.12 Dispatch Gate

```text
[ ] Objective rõ
[ ] Dependency state rõ
[ ] allowed/readonly/forbidden rõ
[ ] shared resources đã cấp phát
[ ] expected outputs rõ
[ ] acceptance commands discover được
[ ] stop/DR conditions rõ
[ ] isolated workspace có thật
[ ] base SHA được ghi
```

Nếu một mục critical là `UNKNOWN`, WP chưa `READY`.