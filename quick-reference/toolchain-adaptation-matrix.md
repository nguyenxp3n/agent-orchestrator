# Quick Reference: Toolchain Adaptation Matrix

| Evidence need | Node/Web | Go | Python | Rust | .NET | Generic fallback |
|---|---|---|---|---|---|---|
| Install | npm/pnpm/yarn install | go mod download | pip/uv/poetry | cargo fetch/build | dotnet restore | project docs/CI |
| Unit tests | npm/pnpm test | go test ./... | pytest | cargo test | dotnet test | discovered command |
| Lint/type | eslint/biome/tsc | golangci-lint/go vet | ruff/mypy | clippy | analyzers | CI/task runner |
| Global gate | npm script/task | Taskfile/Makefile | tox/nox/task | cargo + task | solution scripts | canonical CI command |

## Isolation Matrix

| Environment | Preferred | Fallback |
|---|---|---|
| Git local | worktree | separate clone |
| Cloud IDE | workspace/branch | separate project workspace |
| High-risk tools | container/VM | sandbox + serialized execution |
| Low RAM | fewer concurrent agents | remote service/in-memory tests |

## Rule

Không chọn command chỉ từ bảng này. Bảng là heuristic. Project Execution Profile lấy authority từ repository/CI thực tế.