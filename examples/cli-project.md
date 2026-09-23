# Example: Command-Line Developer Tool

## Objective
Implement new command subtrees and flag parsing without concurrent modifications to the root command dispatcher.

## Dependency DAG
```text
WP-100 Command Flag Spec Freeze
  +--> WP-210 Config Subcommand -------+
  +--> WP-220 Build Subcommand --------+--> WP-400 CLI E2E Verification
```

## Boundary and Resource Allocation
```text
WP-100: specs/cli-spec.md (Frozen command syntax)
WP-210: pkg/cmd/config/**, Flag namespace `--config-*`
WP-220: pkg/cmd/build/**, Flag namespace `--build-*`
```

The root command registry (`cmd/root.go`) is designated `INTEGRATION_ONLY`.

## Verification Protocol
Workers execute unit tests against subcommands using mock terminal streams. The Integrator registers approved subcommands into the root dispatcher and validates automated CLI help generation and end-to-end command tests.