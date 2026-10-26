# Stage 0 — Product definition complete

**Status:** complete. This milestone establishes the product baseline and implementation boundaries. It does not certify implemented behavior, select the technical stack, or grant a software license.

## Product statement

Moai — Mother of All Infra — is an open, self-hostable infrastructure platform designed to deploy and operate applications, connect or manage dependencies, coordinate infrastructure changes, and support evidence-based assistance through replaceable tools and models.

The initial operator is a developer or small team managing containerized applications on one supported Linux VPS. The first release uses prebuilt images, supports essential services and jobs, and includes recovery, basic alerts/analytics, and read-only MCP. Broader source builds, migration, and AI capabilities follow defined stages.

## Requirements traceability

| ID | Requirement | Design evidence | Delivery and acceptance |
| --- | --- | --- | --- |
| R01 | Open-source core and dependencies; no mandatory proprietary service | [Policy](open-source-and-standards.md) | All stages; license, dependency inventory, SBOM, and self-hosted verification before executable release. |
| R02 | Start on one VPS; support multiple projects and languages through workload packaging | [Vision](vision.md), [first release](first-release.md) | Stages 1–3; deploy a pinned image and isolate project configuration. |
| R03 | Replace providers, services, tools, and dashboard integrations through contracts | [Architecture](architecture.md) | Contracts in Stage 1; built-in adapters first; expansion in Stage 10 with capability tests. |
| R04 | Connect existing services and distinguish them from managed resources | [Architecture](architecture.md#managed-and-external-services) | Stage 3; validate bindings and deny unsupported lifecycle actions. |
| R05 | Coordinate deployments, workers, scheduled jobs, and later rollout strategies | [First release](first-release.md) | Stages 2–5; restart/overlap and recovery tests; canary delivery in Stage 10. |
| R06 | Safe backup, restore, and supported infrastructure migration | [Migration](migration.md) | Restore in Stage 5; migration in Stage 7 with fault scenarios and measured interruption. |
| R07 | Configurable local/hosted LLMs, per-task settings, and explicit fallbacks | [AI operations](ai-operations.md#configurable-llms) | Stage 8; profile/capability checks, self-hosted investigation, and prohibited fallback tests. |
| R08 | LLM-friendly contracts and inbound/outbound MCP | [Agent interfaces](agents-events-and-analytics.md) | Schemas in Stage 1; read-only MCP in Stage 4; external tools in Stage 8; controlled mutations in Stage 9. |
| R09 | Alerts and hooks across resources, workflows, and custom events | [Events and alerts](agents-events-and-analytics.md) | Basic rules in Stage 4; broad hooks/adapters in Stage 10; test duplicates, missing data, delivery failure, and loops. |
| R10 | Shared operational analytics, later cost and application analytics | [Analytics](agents-events-and-analytics.md#analytics-as-a-shared-capability) | Operational summaries in Stage 4; richer analytics in Stage 10 with scope and query-budget checks. |
| R11 | Git and repository hosting must be replaceable | [Sources and artifacts](sources-and-artifacts.md) | Direct artifact deployment in Stage 2; Git and non-Git adapter evidence in Stage 6. |
| R12 | Support/monitoring evidence leads to diagnosis and permitted actions | [AI operations](ai-operations.md) | Investigation in Stage 8; validated, approved execution in Stage 9. |
| R13 | Configurable dashboards and an open extension ecosystem | [Vision](vision.md#customize-the-experience), [architecture](architecture.md#extension-contracts) | Minimal dashboard in Stage 2; third-party isolation and customization in Stage 10. |

## Exit checks

- [x] Project identity, intended users, and example journey documented.
- [x] Core requirements mapped to design documents and delivery stages.
- [x] Open-source and standards requirements recorded as project policy.
- [x] Provider, source-control, and LLM configurability explicitly covered.
- [x] First-release inclusions, exclusions, and acceptance evidence defined.
- [x] Migration, background-job, authorization, and recovery limits stated.
- [x] Product baseline recorded in an [accepted decision](decisions/0001-product-baseline.md).
- [x] Open implementation decisions listed with resolution deadlines.
- [x] Local documentation links, anchors, headings, and code fences validated.

## Handoff to Stage 1

The [engineering register](engineering.md#open-decision-register) is the implementation decision backlog. Resolve stack, state persistence, host access, authentication, and secrets before their dependent components. Specification revisions and adapter compatibility require implementation evidence. The exact OSI-approved license must be selected before public code release; this milestone publishes documentation only and makes no license grant.

The next deliverable is a runnable local core that can create a project, validate and record an operation plan, and recover its state after restart. It must include meaningful validation and authorization checks. Host deployment follows in Stage 2.

Repository initialization and publication record this baseline. They do not mark Stage 1 or any implemented capability complete.
