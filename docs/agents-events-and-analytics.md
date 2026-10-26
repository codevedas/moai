# LLM interfaces, events, alerts, and analytics

**Status: required product direction with proposed contracts.** No MCP server, event system, or analytics backend is implemented yet.

## A platform agents can understand and operate

Moai must expose structured infrastructure context and discoverable operations to external assistants and its own AI integrations. Agent access is a first-class interface alongside the dashboard and API. Ordinary operation remains available without an LLM.

The inference provider and model are independently configurable through [model profiles](ai-operations.md#configurable-llms). MCP tool connections do not dictate the model, and changing models does not grant additional infrastructure permissions.

Resources need stable identifiers, typed schemas, dependency relationships, timestamps, provenance, and links to relevant releases and operations. Tools need clear input/output contracts, capability requirements, side effects, structured errors, and verification results. Queries must support filtering, pagination, and bounded output so an agent can request relevant context without exporting an entire project.

Publish human-readable and machine-readable interface documentation, examples, and compatibility information. Equivalent operations from the dashboard, API, or agent interface must use the same authorization and workflow implementation.

## MCP in both directions

Use the [Model Context Protocol](https://modelcontextprotocol.io/specification/2025-11-25) for standardized agent integration. MCP defines resources, tools, and prompts; Moai should expose the applicable features and negotiate supported capabilities. The implementation must select and document its supported protocol revision and transports.

- **Moai as an MCP server:** authorized clients can inspect projects, topology, health, alerts, analytics summaries, and operation history; request plans; and invoke permitted operations.
- **Moai as an MCP client:** operators can connect approved external MCP servers for context or capabilities such as observability, support systems, and other tools. These connections are optional integrations.
- **Reusable prompts:** optionally expose investigation and planning templates that refer to current authorized resources rather than embedding credentials or stale infrastructure snapshots.

Tool names and schemas remain a design decision. Separate inspection, planning, approval, and execution capabilities. A model must not grant itself approval. Mutations must reference the approved plan revision where required and return a durable operation identifier for status and recovery. MCP tool calls and client reconnections must not create duplicate operations.

An MCP connection is not blanket trust. Scope identity and credentials to projects and capabilities, validate remote responses, and apply the same data handling rules to external servers as to other integrations. Protocol metadata and tool annotations cannot replace server-side enforcement. Resource reads and analytics queries require authorization too.

## Events and hooks throughout the system

Applications, services, hosts, deployments, workers, scheduled jobs, backups, migrations, plugins, and AI workflows should expose documented event points. Operators can also ingest custom application events and external webhooks through authenticated adapters.

Use a [CloudEvents](https://cloudevents.io/) envelope for exchanged events. Define Moai payload schemas and scoped references for project, environment, resource, operation, correlation, severity, and evidence. Preserve event identity and origin; authenticate the producer rather than trusting a submitted project identifier.

Separate these concepts:

- An **event** records something that happened.
- An **alert rule** evaluates events, metrics, logs, or query results.
- An **alert instance** tracks a condition and its lifecycle.
- A **notification** delivers an alert or update to a configured destination.
- A **workflow trigger** requests an operation subject to policy.

CloudEvents standardizes the envelope; persistence and delivery behavior need their own implementation decisions. Consumers must handle duplicate and out-of-order delivery, bounded retries, replay, and failed deliveries. Alert-triggered actions need loop prevention, deduplication, and rate limits.

## Configurable alerts at every supported scope

Operators should be able to attach rules to platform, project, environment, resource, and workflow scopes. This is the meaning of alerts being available anywhere: documented extension points and configurable bindings across the system, including custom events.

Rules should support thresholds, time windows, missing-data behavior, severity, grouping, maintenance suppression, cooldowns, acknowledgement, resolution, and escalation. Effective rules and inherited settings must be inspectable. Global rules still need authorization for the resources they observe.

Notifications should route through replaceable adapters to the dashboard, webhooks, email, or operator-selected services. Credentials and destinations must be configured explicitly. A delivery failure must be visible independently of the original alert. Alerts remain useful when no AI model is connected.

## Analytics as a shared capability

Support operational analytics such as latency, errors, resource consumption, deployment outcomes, backup freshness, queue delay, incidents, and recovery times. Add cost analytics where provider data exists. Optional application/business events can support product-specific dashboards and correlations without making Moai a mandatory collector of customer data.

Keep instrumentation, storage/query engines, rules, and visualization behind separate contracts. Operators can use supported existing analytics systems or self-hosted implementations. The dashboard, MCP clients, alerts, and investigations should access the same authorized query layer.

Results must carry time range, units, source, freshness, aggregation, and missing-data information. Distinguish observed measurements from estimates. Define retention, redaction, query limits, and cardinality budgets; cross-project aggregation requires explicit access. Expensive or high-volume queries must not overwhelm the single-VPS deployment.

## Example workflow

A worker emits a job-failure event. A project rule groups repeated failures and opens an alert. A dashboard widget shows the error rate alongside the latest deployment. An authorized assistant queries the related evidence through MCP and proposes a compatible release rollback. After policy checks and any required approval, the workflow engine applies it, verifies recovery, and updates the alert. Notification adapters deliver the configured updates.

Every connection in this example is replaceable or configurable. The same event can feed an analytics backend, a notification destination, or an investigation without coupling all three to one vendor.

## Delivery and acceptance

Start with structured resources, operation events, a small read-only MCP surface, basic alert rules, and operational summaries. Add external MCP connections, richer analytics, notification adapters, and approved mutation tools as their underlying capabilities become reliable.

Test client capability negotiation, scoped discovery and reads, invalid inputs, bounded queries, disconnected clients, duplicate requests/events, out-of-order events, failed notification delivery, alert storms, trigger loops, redaction, and denied cross-project access. Verify that unavailable models and optional integrations do not interrupt core deployment or alert evaluation. Advanced hooks, business analytics, and integrations remain staged work rather than implied first-release features.
