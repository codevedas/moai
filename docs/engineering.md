# Engineering standards and open decisions

**Status: proposed working standards, subject to the required [open-source and open-standards policy](open-source-and-standards.md).** No implementation language or framework has been selected.

## Stack selection

Choose the stack through small prototypes of the most consequential operations: host access, durable execution, interruption recovery, service inspection, backup/restore, and a minimal dashboard.

First verify open-source licensing, dependency compatibility, and a self-hostable build/runtime path. Then evaluate maintainability, contributor familiarity, deployment footprint, dependency support, distribution, debugging, and failure handling. Supporting applications written in multiple languages does not require implementing the platform in multiple languages.

Use a small number of implementation technologies initially. Add a separate language or service when measured constraints justify its operational cost.

## Proposed standards

- Version public resource schemas, API contracts, configuration formats, and extension interfaces explicitly.
- Specify APIs in a machine-readable form appropriate to the chosen protocol; use OpenAPI if HTTP APIs are selected.
- Use immutable release references and record configuration revisions for reproducibility.
- Return structured errors and operation identifiers; distinguish retryable failures from uncertain side effects.
- Use timeouts, bounded retries, and idempotency mechanisms appropriate to each operation.
- Keep secret values out of source control, logs, plans, and ordinary API output.
- Emit structured logs and correlation identifiers; use OpenTelemetry and OTLP at applicable telemetry boundaries, with exact versions and conventions selected during implementation.
- Validate user input, adapter output, and model-produced plans at trust boundaries.
- Maintain compatibility matrices for supported hosts, runtimes, services, and adapters.
- Separate application rollback, schema migration, data restoration, and infrastructure migration in APIs and documentation.

These are project standards to validate during implementation, not a claim of compliance with an external certification.

## Verification strategy

Use unit tests for state transitions and policy decisions, adapter contract tests for capability behavior, and integration tests against real supported services. End-to-end scenarios should exercise user outcomes such as deployment, failed rollout recovery, restore, and eventually migration.

Failure injection is central to operation reliability: terminate processes, interrupt connections, fill disks, and restart stale workers in controlled test environments. Verify resulting infrastructure state as well as returned status.

Benchmark control-plane overhead on a representative small VPS before publishing minimum hardware requirements. Repeat compatibility testing when expanding supported versions.

## Decision process

Record consequential decisions in `docs/decisions/` when made. Each decision should include context, alternatives, the chosen approach, consequences, and conditions that would justify revisiting it. The [product baseline](decisions/0001-product-baseline.md) records the Stage 0 constraints; implementation decisions remain open below.

## Open decision register

| Decision | Must be resolved before |
| --- | --- |
| Technical namespace and package identifiers | Publishing stable identifiers or packages. |
| Specific OSI-approved project license and dependency compatibility review | Initial public code release; the open-source policy is already required. |
| Core implementation language and framework | Executable prototype. |
| Dashboard implementation and API protocol | UI/API implementation. |
| Control-state persistence and durable workflow approach | Recoverable operations. |
| Host execution transport and credential model | Host registration. |
| Supported OS, CPU architecture, and runtime | Claiming host compatibility. |
| Routing and TLS management | Public application deployment. |
| Authentication, authorization, and project boundaries | Exposing the control API. |
| Secret storage, encryption, rotation, and bootstrap recovery | Storing infrastructure credentials. |
| Backup storage, encryption, retention, and recovery procedure | Backup support. |
| Scheduler ownership and fencing mechanism | Reliable scheduled jobs and migration. |
| Initial database and Valkey versions and configurations | Service provisioning. |
| Plugin packaging, trust, and isolation | Loading third-party extensions. |
| Model data handling and retention defaults | External AI integration. |
| Model profile schema, capability checks, scope inheritance, budgets, and fallback rules | Configurable LLM inference. |
| MCP revision, transports, identity mapping, and tool/resource schemas | MCP integration. |
| Event persistence, delivery, payload schemas, and alert-rule semantics | Event-driven alerts and workflow triggers. |
| Analytics query contracts, storage, retention, and resource budgets | Shared analytics and custom event ingestion. |
| Source snapshot contract, build isolation, artifact provenance, and retention | Source/build integrations; direct artifact deployment must remain independent. |
| Governance and contribution process | Accepting community contributions at scale. |

## Documentation discipline

Update documentation with behavior changes. Label proposals and unsupported combinations clearly. Installation commands, URLs, performance numbers, and compatibility promises must be backed by working artifacts or verification; do not add placeholders that appear executable or authoritative.
