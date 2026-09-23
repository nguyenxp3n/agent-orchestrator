# Scenario: Path Ownership Collision

## Trigger
Hai WPs yêu cầu write vào cùng exclusive path/hotspot.

## Risk
Overwrite, merge conflict, ownership ambiguity.

## Evidence
Exact allowed paths; planned edits; repository hotspot list.

## Immediate Action
Reject parallel plan; split hotspot thành Integration Request hoặc serialize/transfer ownership.

## Forbidden Response
Không bảo agents “cẩn thận đừng sửa cùng dòng” như cơ chế chính.

## Recovery Procedure
Recompile WP boundaries và workspaces.

## Exit Criteria
Chỉ một active writer cho region; plan mới có audit boundary rõ.

## Example Lead Response
```text
Collision tại services/api/router.go: chuyển file thành INTEGRATION_ONLY và để workers chỉ xuất route modules.
```
