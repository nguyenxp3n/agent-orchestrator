# Pre-Dispatch Checklist

Complete this checklist before dispatching any Work Package to an active coding worker.

## 1. Prerequisites and dependencies
- [ ] Upstream hard dependencies in the DAG are merged or frozen.
- [ ] Required interface contracts (OpenAPI, Protobuf, shared types) are frozen in `readonly_paths`.
- [ ] Project Execution Profile has verified that all required build and test tools exist in the environment.

## 2. Boundary and resource isolation
- [ ] An isolated workspace (Git worktree, clone, or container) is provisioned.
- [ ] Base commit SHA is verified and recorded.
- [ ] `allowed_paths` strictly covers the assigned domain with zero overlap against active concurrent workers.
- [ ] `forbidden_paths` protects central routers, bootstrap entrypoints, CI configs, and other workers' files.
- [ ] Shared semantic resources (migration sequence numbers, ports, route patterns) are explicitly allocated.

## 3. Contract completeness
- [ ] Measurable objective and boolean Success Predicate are defined.
- [ ] Non-counting outcomes are specified to block superficial or incomplete implementations.
- [ ] Exact project test commands and quality gate scripts are specified.
- [ ] Expected deliverable files are enumerated.
- [ ] Stop conditions and escalation paths are explicit.

## 4. Quality gate disposition
- [ ] Evaluated against the Prompt Quality Gate.
- [ ] Gate disposition is confirmed as `READY`.