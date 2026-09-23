# Scenario: Resource / Migration Collision

## Trigger
Hai WPs xin cùng migration number, port, route hoặc DB object.

## Risk
Runtime conflict, migration sequence break, semantic corruption.

## Evidence
Resource Allocation Registry; current migrations; active leases.

## Immediate Action
Block second allocation; giữ owner hợp lệ và cấp slot khác.

## Forbidden Response
Không tự lấy “next number” trên branch riêng.

## Recovery Procedure
Update resource registry, candidate references và re-audit candidate đã đổi.

## Exit Criteria
Không còn duplicate exclusive resource và ordering hợp lệ.

## Example Lead Response
```text
Resource slot R2 đã thuộc WP-B; WP-INFRA không được tự claim R2 hoặc tạo shared identifier khác. Nếu project dùng DB, R2 có thể là migration slot; nếu không, dùng resource type thực tế.
```
