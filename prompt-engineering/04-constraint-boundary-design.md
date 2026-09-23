# 04: Constraint & Boundary Design

## Mục tiêu

Constraints phải bảo vệ invariants và quyền sở hữu, không biến prompt thành danh sách cấm dài vô tận.

## Path boundaries

```text
ALLOWED_PATHS    = vùng được write/delete/rename
readonly_paths / READONLY_PATHS = được đọc để hiểu, không sửa
forbidden_paths / FORBIDDEN_PATHS = tuyệt đối không chạm trong WP hiện tại
```

Nếu project không dùng filesystem ownership, thay bằng equivalent domain boundaries: package/module/service/schema/resource.

## Semantic resources

Path isolation không đủ cho shared resources. Compile allocation cho những loại thực sự tồn tại trong project:

- migration/version slot;
- network port;
- route/endpoint namespace;
- database table/schema;
- event/topic name;
- environment variable namespace;
- feature flag;
- deployment target;
- shared generated registry.

Không invent resource type chỉ vì framework có ví dụ.

## Constraint taxonomy

### Hard invariant

Vi phạm làm invalid work:

- không sửa forbidden paths;
- không tự allocate shared resource;
- không đổi frozen public contract;
- không dùng secret khác trust domain;
- không merge protected branch.

### Quality constraint

Định nghĩa bar:

- theo project conventions;
- tests phải chứng minh behavior;
- maintain backward compatibility nếu contract yêu cầu.

### Preference

Có thể linh hoạt nếu trade-off tốt hơn:

- naming style ở local helper;
- cách chia internal function;
- implementation technique nằm trong ownership.

Không nâng preference thành hard rule nếu không có lý do.

## Explain the why khi hữu ích

Một constraint khó đoán nên kèm rationale ngắn:

```text
Do not reuse the staging signing secret in test environments because the trust domains must remain isolated.
```

Rationale giúp model generalize khi gặp case chưa liệt kê.

## Scope extension

Nếu task bắt buộc vượt `allowed_paths` hoặc resource allocation:

```text
STOP affected change
-> describe necessity
-> identify requested path/resource
-> explain architectural impact
-> send Decision/Resource/Integration Request
```

Không “sửa tạm rồi báo sau”.
