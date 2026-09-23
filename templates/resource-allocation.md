# Template: Resource Allocation Registry

| Resource ID | Resource Type | Owner WP | Mode | Value / Sequence Slot | Status | Notes |
|---|---|---|---|---|---|---|
| RES-MIG-01 | DB Migration | WP-210 | ALLOCATED_WRITE | `000021_create_reviews.sql` | RESERVED | Assigned to auth domain |
| RES-MIG-02 | DB Migration | WP-220 | ALLOCATED_WRITE | `000022_add_ratings.sql` | RESERVED | Assigned to review domain |
| RES-PORT-01| Network Port | WP-300 | EXCLUSIVE_WRITE | `8081` | ACTIVE | Auth service local port |
| RES-ROUTE-1| API Route | WP-100 | FROZEN_CONTRACT | `/api/v1/reviews/*` | FROZEN | Defined in OpenAPI spec |
| RES-ENV-01 | Env Variable | WP-210 | EXCLUSIVE_WRITE | `AUTH_SERVICE_JWT_SECRET` | RESERVED | Secret policy applied |