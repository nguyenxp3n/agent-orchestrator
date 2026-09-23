# Template: Code Ownership Matrix

| WP_ID | Owner Role | Allowed Paths (Write/Delete) | Read-Only Paths | Integration-Only Hotspots | Assigned Resources |
|---|---|---|---|---|---|
| WP-100 | Architect | `packages/contracts/**` | `docs/**` | None | Frozen API schemas |
| WP-210 | Worker-1 | `services/auth/**` | `packages/contracts/**` | `services/api/router.go` | Migration Slot R1 |
| WP-220 | Worker-2 | `apps/web/src/features/auth/**`| `packages/contracts/**` | `apps/web/src/routes.tsx` | Route namespace `/auth` |
| WP-300 | Worker-3 | `deploy/**`, `infra/**` | `services/**` | Root docker-compose | Ports 8080, 5432 |