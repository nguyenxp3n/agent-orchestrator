# Quick Reference: Toolchain Adaptation Matrix

| Architecture Type | Canonical File Markers | Common Build Systems | Typical Quality Gates | Common Hotspots |
|---|---|---|---|---|
| **Go / Microservices** | `go.mod`, `go.sum` | `go build`, `make` | `go test -v ./...`, `golangci-lint` | `cmd/server/main.go`, proto schemas |
| **Node.js / Fullstack** | `package.json`, lockfile | `pnpm`, `npm`, `yarn` | `pnpm lint`, `pnpm typecheck`, `pnpm test` | Root router, central `package.json` |
| **Python / Fast-API** | `pyproject.toml`, `setup.py` | `poetry`, `uv`, `pip` | `ruff check`, `mypy .`, `pytest` | App entrypoint, Alembic migrations |
| **Rust / Systems** | `Cargo.toml`, `Cargo.lock` | `cargo` | `cargo check`, `cargo clippy`, `cargo test` | `src/main.rs`, shared crates |
| **Java / Kotlin** | `pom.xml`, `build.gradle` | `maven`, `gradle` | `mvn test`, `gradle test` | Application context, Flyway scripts |

The Lead discovers the actual toolchain from repository truth. Never force foreign tools onto an existing repository.