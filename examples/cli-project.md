# Example: CLI Tool

## Intake

Xác định command tree, parser library, public flags, config format, filesystem side effects và supported platforms.

## Work Packages

```text
WP-CMD-IMPORT -> import subcommand
WP-CMD-EXPORT -> export subcommand
WP-CONFIG     -> config validation
WP-DOCS       -> usage/reference
ROOT CLI REGISTRATION -> INTEGRATION_ONLY if multiple workers add commands
```

## Ownership

Public flag names và exit-code semantics là semantic resources. Hai workers không được tự chọn cùng `--format` với nghĩa khác nhau.

## Verification

```bash
pytest
# or cargo test / go test ./... according to project
<cli-smoke-command> --help
```

Audit phải kiểm actual stdout/stderr/exit codes và generated files, không chỉ unit tests.