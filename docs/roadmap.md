# Development stages

**Stage 0 is complete:** see the [completion record](stage-0.md) and [accepted product baseline](decisions/0001-product-baseline.md). Implementation has not started. These stages define the development sequence and completion evidence; they make no calendar or staffing commitment.

| Stage | Deliverable | Completion evidence |
| --- | --- | --- |
| 0. Product definition — complete | Vision, open-source policy, configurable components, MCP, events, analytics, and source independence. | Requirements traced to design docs and delivery stages; first-release boundaries, acceptance criteria, and open decisions recorded. |
| 1. Runnable core | Stack decisions, local development setup, projects/environments, resource contracts, configuration revisions, authorization, secrets, and durable operation records. | Start the service, create a project, inspect a plan, restart, and recover recorded state; test validation and access boundaries. |
| 2. One-host deployment | Host registration, an OCI runtime adapter, pinned-image deployment, routing, health checks, logs, and a minimal dashboard. | Deploy a sample application without Git or repository credentials; diagnose a failed rollout on a controlled test host. |
| 3. Services and jobs | PostgreSQL, optional Valkey, external bindings, workers, schedules, and dependency tracking. | Run an application with persistent data and background work; verify restart/overlap behavior and reject unsupported service actions. |
| 4. Visibility and MCP | Structured events, basic alert rules, operational summaries, OpenTelemetry, and read-only MCP. | An authorized client reads scoped health/history; alerts fire and resolve without an LLM; test missing data, duplicates, and access denial. |
| 5. Reliability and first release | Backups, verified restore, compatible release recovery, interrupted-operation reconciliation, and installation/upgrade/recovery docs. | Pass all [first-release criteria](first-release.md), select the license, review dependencies, publish an SBOM, and verify the self-hosted path. |
| 6. Source and build adapters | Isolated builds, one Git adapter, one non-Git source adapter, change notifications, and retained artifacts. | Both adapters use the same build/deploy contracts; roll back from retained artifacts while source access is unavailable. |
| 7. Controlled migration | Supported VPS-to-VPS move, data transfer, worker drainage, scheduler ownership, source fencing, routing, and recovery. | Pass [migration failure scenarios](migration.md); publish measured interruption and recovery limits. |
| 8. Configurable AI investigation | Local/hosted model profiles, task selection, budgets, permitted fallbacks, external MCP connections, and support/monitoring investigation. | Switch supported models without changing infrastructure adapters; demonstrate self-hosted investigation and reject prohibited data sharing or malicious instructions in evidence. |
| 9. Approved automation | Policy-bound mutation tools, plan-specific approval, verification, and bounded remediation for validated operation types. | Reject stale/unauthorized plans; prevent duplicate actions on retries; verify recovery and unchanged authority when models change. |
| 10. Ecosystem and scale | More providers/runtimes, plugin distribution, advanced hooks, notification adapters, cost/application analytics, custom dashboards, canary delivery, and multi-host support. | Each integration passes capability, compatibility, isolation, and failure checks; measure before publishing capacity claims. |

## Foundation scope

Stage 1 selects and records the implementation language/framework, state persistence, host execution transport, supported host environment, authentication, and secret handling. It introduces stable resource identities, capability declarations, provider-neutral release/artifact contracts, and structured event envelopes.

Start with built-in adapters and a small deployable core. Separate processes and third-party plugin loading follow demonstrated needs. The first working slice is a persisted operation lifecycle; the next is a complete application deployment on one supported host.

## Release boundaries

Stages 2–5 produce the first useful operational release. A deployment demo does not establish production readiness: restore, interruption recovery, authorization, and documented limits are part of the release gate.

Source builds follow the [source and artifact contracts](sources-and-artifacts.md). Direct image deployment remains available without a source provider. Supporting another provider does not automatically include migrating its history or reviews.

Migration support applies only to tested configurations. AI assistance uses the [configurable model profiles](ai-operations.md#configurable-llms), while MCP clients and model choice remain independent. Automation cannot exceed the verified capabilities of the underlying operation engine.

## Sequencing and validation

Extensibility, open interfaces, LLM-friendly schemas, and source-provider independence begin in Stage 1; every integration need not be implemented then. The first release includes a small read-only MCP surface and basic alerts/analytics. Richer integrations are staged as described in [agents, events, and analytics](agents-events-and-analytics.md).

Source builds, migration, and AI can be independently prototyped once their prerequisites exist, but must not displace first-release recovery gates. Re-estimate effort after each working slice instead of committing to a schedule before testing difficult operations. Update the current status only when executable evidence supports it.
