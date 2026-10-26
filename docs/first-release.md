# First release

**Status: proposed scope.** The purpose of the first release is to prove reliable application operations on a constrained, documented configuration.

## Target operator and environment

One trusted operator manages one Linux VPS and one or more containerized applications. The control plane may run on the same machine. The supported operating system, CPU architecture, container runtime version, and minimum resources must be selected and tested before implementation is presented as supported.

The initial delivery input is a prebuilt container image pinned by digest. Deployment must work without Git or repository credentials. Source builds, provider-specific change notifications, and build infrastructure are later adapters following the [source and artifact contracts](sources-and-artifacts.md).

## Included scope and acceptance evidence

| Capability | Evidence required |
| --- | --- |
| Host registration | Validate access and prerequisites; report incompatible hosts without partial installation being mistaken for success. |
| Application deployment | Deploy an immutable image reference, inject secret references safely, configure routing, and pass an application health check. |
| Project configuration | Persist versioned environment settings and dependency bindings; keep secret values out of ordinary responses and logs. |
| Managed PostgreSQL | Provision a pinned supported version with persistent storage and a documented backup/restore workflow. |
| Optional Valkey | Deploy a supported configuration and explicitly classify it as disposable cache or persistent application state. |
| External service binding | Connect to an existing supported service and show its restricted management capabilities. |
| Workers and jobs | Run configured background processes and scheduled tasks with documented stop, restart, retry, and overlap behavior. |
| Release recovery | Detect an unhealthy release and restore the previous compatible application release; explain when database changes prevent rollback. |
| Operational visibility | Show deployment status, recent logs, health, and durable operation history with actionable failures. |
| Read-only MCP | An authorized client can discover and read scoped resource health and operation summaries; verify bounded results and denied cross-project access. |
| Basic alerts and analytics | Evaluate health/deployment rules, show firing/resolved alerts in the dashboard, and provide operational summaries with time ranges and freshness; verify duplicate-event handling and missing-data behavior. |
| Backup and restore | Restore a backup to a clean target, validate application data, and record the artifact and tested recovery procedure. |
| Interrupted execution | Restart the control plane during an operation and recover without silently duplicating completed destructive or externally visible steps. |
| Access control | Require authenticated operator access and protect infrastructure credentials; document bootstrap and credential rotation. |

Routing, TLS handling, backup destination, and the operator authentication mechanism need explicit design choices before this scope is implemented.

## Example acceptance scenario

Deploy a sample application with PostgreSQL, a worker, and a scheduled task. Attach or provision Valkey. Create representative data, deploy a healthy update, and intentionally deploy an unhealthy update. Confirm that the prior compatible release can be restored and that failures are visible.

Create an off-host backup and restore it onto a clean supported environment. Verify representative records and application behavior. Interrupt a deployment and restart the control plane; confirm that the operation either resumes safely or enters an explicit recovery state.

Test failure paths such as insufficient disk space, an unreachable image registry, invalid service credentials, an unavailable backup destination, and a lost host connection. Each must leave an understandable record and a documented next action.

## Deferred from this release

- General-purpose VPS-to-VPS migration; this follows the reliability gate.
- Multi-cloud orchestration, automatic scaling, and Kubernetes execution.
- Canary delivery and sophisticated traffic splitting.
- Automatic application source-code changes.
- Autonomous AI remediation and support inbox integrations.
- Arbitrary third-party plugins or a plugin marketplace.
- Advanced dashboard customization and organization-level multi-tenancy.
- Automatic database version upgrades or cross-engine data conversion.

The core boundaries should permit these capabilities later without requiring them for the first useful release.

## Release gate

Publish an executable release only with a selected license, supported-version matrix, installation and removal instructions, upgrade and recovery instructions, a credential handling design, and evidence for the acceptance scenarios above. Meet the [open-source and standards release checks](open-source-and-standards.md), including dependency review, an SPDX bill of materials, and verification of the self-hosted path. No calendar estimate is committed in this document.
