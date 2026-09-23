# Chương 3: Quản trị tài nguyên và chống xung đột

## 3.1 Permission Envelope cho mỗi Worker

Mỗi worker nhận ba nhóm path:

```yaml
allowed_paths:
  - <owned-domain-path>/**
readonly_paths:
  - api/openapi.yaml
  - docs/architecture/**
forbidden_paths:
  - services/api/main.go
  - .github/workflows/**
```

- `allowed_paths`: write/delete/rename trong contract.
- `readonly_paths`: đọc nhưng không sửa.
- `forbidden_paths`: không chạm; nếu cần, tạo request.

Rename/move tính là delete + create, nên cả source và destination đều phải hợp lệ.

## 3.2 Ownership modes

Agent Orchestrator dùng các mode khái niệm sau:

- `EXCLUSIVE_WRITE`: một active writer.
- `SHARED_READ`: nhiều reader.
- `INTEGRATION_ONLY`: chỉ Integrator/change request đã duyệt.
- `ALLOCATED_WRITE`: chỉ instance được cấp, ví dụ migration slot.
- `APPEND_ONLY`: thêm theo rule, không rewrite lịch sử.
- `GENERATED`: file được sinh, worker không edit trực tiếp.

Không cần runtime lock manager để áp dụng; Lead có thể quản lý bằng Ownership Matrix + ledger miễn allocation rõ và audit được.

## 3.3 Semantic Resources quan trọng hơn path khi cần

Collision có thể xảy ra dù paths không overlap: hai migrations cùng table; hai services cùng port; hai modules cùng route; hai workers định nghĩa cùng env var khác nghĩa; hai consumers dùng cùng event name khác schema.

Resource Registry nên có:

```text
resource_id | type | owner_wp | mode | value/slot | status | notes
```

## 3.4 Migration Slots

Migration là ví dụ điển hình của `ALLOCATED_WRITE`. Lead cấp slot trước dispatch:

```text
R1 -> WP-A / Agent 1
R2 -> WP-B / Agent 2
R3 -> WP-C / Agent 3
R4 -> WP-D / Agent 4
```

Worker không được “lấy số tiếp theo” bằng cách tự nhìn directory, vì các branches song song có thể cùng chọn một số.

```text
You own allocated resource slot R2 only.
Do not allocate R1, R3, R4 or create an additional shared identifier.
If another shared resource becomes necessary, stop and send RESOURCE_REQUEST.
```

## 3.5 Port, env, route và queue allocation

```text
PORT-API-DEV      -> 8081 -> WP/API
PORT-RUNNER-DEV   -> 8092 -> WP/RUNNER
ENV-REVIEW-KEY    -> reserved by security decision, not reusable
ROUTE-/review/*   -> contract owner WP-CONTRACT
QUEUE-ai.jobs     -> event contract owner
```

Secret không copy vào Resource Registry; chỉ record tên/owner/policy, value nằm trong secret manager hoặc environment được kiểm soát.

## 3.6 Integration Hotspots

Các file dễ bị mọi worker muốn sửa: root router, application bootstrap, central DI, `Taskfile.yml`, root package manifest, global schema registry, shared CI workflow.

Đặt chúng `INTEGRATION_ONLY` khi nhiều WPs song song. Worker gửi Integration Request:

```text
Request: register /review routes
Candidate artifact: <owned-domain-path>/<candidate-artifact>
Desired hotspot: services/api/router.go
Reason: accepted WP requires registration
Verification: repository global tests + route contract test
```

Integrator áp dụng sau audit, giảm merge conflict và ownership rộng.

## 3.7 Kỹ thuật chặn đứng từ sớm

Khi agent xin sửa ngoài scope:

1. Xác định file/resource có thực sự cần cho acceptance không.
2. Kiểm ownership/DAG xem ai đang sở hữu.
3. Tìm phương án trong scope.
4. Nếu không có, chọn scope extension có ràng buộc, Integration Request, ownership transfer, resource allocation mới hoặc WP mới.
5. Ghi quyết định.

```text
Không cấp quyền sửa migrations/** hoặc services/api/** cho WP-INFRA.
Hai vùng đang thuộc các WP khác và Migration Slots đã được cấp.
Giữ solution trong deploy/** và services/runner-worker/**.
Nếu health-check bắt buộc cần API wiring, gửi Integration Request; không sửa hotspot trực tiếp.
```

Đây là pattern chung để tránh Infrastructure/Platform Worker thay đổi shared schema, root registration hoặc domain resources “tiện thể”.

## 3.8 Scope extension có kiểm soát

Không phải request nào cũng bị từ chối. Nếu một Worker cần sửa shared build/task configuration để expose deliverable, Lead chỉ mở đúng file/resource cần thiết, kèm invariant:

```text
Granted: <shared-build-or-task-config>
Conditions:
- preserve all existing commands/tasks
- project quality gate must remain green
- contract/spec validation must remain green when applicable
- no unrelated refactor
```

Scope extension tốt phải nhỏ, explicit, reversible và có verification bổ sung.

## 3.9 Security boundary

Resource reuse có thể là lỗi security. Nếu agent hỏi dùng deletion HMAC key cho content review, Lead phải từ chối vì hai security domains khác nhau.

```text
Reuse secret across domains? -> reject by default
Weaken fail-closed behavior?  -> escalate
Broaden privilege?            -> prefer narrower design
```

## 3.10 Lease transfer và crash

Resource thuộc WP, không nhất thiết thuộc process/agent:

```text
WP-B owns resource slot R2
Agent-4 generation 2 crashes
Agent-7 generation 3 resumes WP-B
R2 remains WP-B's slot
Agent-4 generation 2 returning later is stale
```

Không recycle migration identifier chỉ vì agent cũ biến mất.

## 3.11 Boundary Audit

```bash
git diff --name-status <base>...HEAD
git status --short
```

Rà changed path với allowed/readonly/forbidden. Sau đó kiểm semantic diff: routes, migration identifiers, DB objects, env vars, ports. Chỉ nhìn filename là chưa đủ.

## 3.12 Nguyên tắc cuối chương

Parallelism an toàn dựa trên **ownership explicit + resource allocation trước + isolation + audit sau**. Khi số agent tăng, Lead phải thu hẹp boundary và tách shared hotspots khỏi worker branches khi có thể.