# Example: CLI Tool

## Intake

Identify the command tree, parser library, public flags, config format, filesystem side effects, and supported platforms.

## Work Packages

```text
WP-CMD-IMPORT -> import subcommand
WP-CMD-EXPORT -> export subcommand
WP-CONFIG     -> config validation
WP-DOCS       -> usage/reference
ROOT CLI REGISTRATION -> INTEGRATION_ONLY if multiple workers add commands
```

## Ownership

Public flag names and exit-code semantics are semantic resources. Two workers must not independently assign different meanings to the same `--format` flag.

## Verification

```bash
pytest
# or cargo test / go test ./... according to project
<cli-smoke-command> --help
```

The audit must verify actual stdout/stderr/exit codes and generated files, not only unit tests.