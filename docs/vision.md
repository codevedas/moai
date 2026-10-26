# Vision and product requirements

## Purpose

Moai stands for **Mother of All Infra**. It is intended to be an open, modular infrastructure management platform that helps operators deploy applications, manage dependencies, investigate problems, and evolve infrastructure through controlled workflows.

The desired experience spans the application lifecycle: from a first deployment on one server to changes in capacity, service composition, and hosting. The platform maintains enough context about applications and their dependencies to explain and coordinate those changes.

## Who it serves

The initial user is a developer or small team operating multiple applications with limited dedicated operations capacity. Their projects may use different languages, databases, queue systems, and hosting providers.

Later versions may serve teams needing shared access, richer policies, multiple servers, and organization-wide integrations. Those needs should inform boundaries without making a single-server installation unnecessarily complex.

## Core requirements

### Start small

The platform must be able to run on one supported VPS or cloud VM alongside application workloads. It should expose resource consumption and avoid requiring a distributed cluster merely to deploy a small application. Actual minimum hardware requirements remain to be measured.

### Bring existing infrastructure

Operators should be able to connect an existing service, such as a Redis instance on another server. A service connection must distinguish platform-managed resources from externally managed resources. The platform should show available operations and access limits for each.

### Choose and replace tools

Providers, service implementations, observability systems, and AI models should be selectable through explicit contracts. Replacing a tool requires compatibility checks and, where applicable, application changes and data conversion. The UI must surface those requirements rather than promise arbitrary transparent swaps.

LLMs must support named, configurable profiles for local or hosted providers, endpoints, models, supported generation settings, usage limits, and data policies. Projects and tasks can select different profiles within administrator limits. Fallbacks are explicit, and AI can be disabled independently of core operations. See [LLM configuration requirements](ai-operations.md#configurable-llms).

### Choose source and build providers

Source-control and repository-hosting choices must also be replaceable. Operators can deploy prebuilt artifacts, use Git through an adapter, or connect another supported source system. Core releases reference immutable artifacts and optional provenance rather than requiring branches or Git commits. See [sources and artifacts](sources-and-artifacts.md).

### Operate multiple projects

Each project needs its own applications, environments, dependencies, configuration, secrets, and operational history. Projects may share infrastructure when explicitly configured. The system must identify shared dependencies before proposing changes that could affect other projects.

### Configure delivery

Operators should eventually be able to select rollout strategies, approval policies, health gates, and environment-specific settings. The first release supports a simple, documented strategy; canary and more advanced traffic management require later capabilities.

### Evolve infrastructure with evidence

Monitoring signals and support reports should lead to investigations tied to concrete services and releases. The system should explain suspected causes, recommend changes, and show cost or resource implications where evidence is available. Infrastructure changes alone may not resolve problems that require application code changes.

### Coordinate migration

A migration should account for persistent data, application versions, configuration, traffic, workers, scheduled jobs, and external integrations. Operators need a plan, an estimated interruption window, verification steps, and a recovery path. Guarantees apply only to explicitly supported configurations.

### Customize the experience

Dashboard widgets, layouts, presets, and integrations should be extensible. UI customization must respect the same permissions as the underlying APIs. A custom widget does not gain additional authority over infrastructure.

### Cross-cutting agent access, alerts, and analytics

Moai must be LLM-friendly throughout its resource model and operation interfaces. MCP should expose authorized context and actions to external agents and allow optional connections to external tools. Alerts and hooks should be configurable across projects, resources, and workflow stages, including custom application events. Analytics should feed dashboards, alert rules, and investigations through shared, replaceable interfaces. See [agents, events, and analytics](agents-events-and-analytics.md) for contracts and delivery boundaries.

## Example journey

1. An operator registers a VPS and creates a project.
2. They deploy an application image with PostgreSQL and a worker.
3. They attach an existing Valkey service and configure a scheduled task.
4. A monitoring alert or support report starts an investigation.
5. The platform correlates the report with relevant telemetry and deployment history.
6. It proposes a specific change, explains uncertainty, and requests any required approval.
7. A durable workflow applies the approved plan and verifies the result.
8. Later, the operator previews and performs a supported move to another VPS.

This journey expresses the long-term experience. It is not the acceptance boundary of the first release.

## Product boundaries

- No promise of zero downtime across arbitrary databases, providers, or application designs.
- No assumption that queue consumers or scheduled tasks provide exactly-once external effects.
- No requirement for a particular deployment product to be a mandatory dependency.
- No requirement that applications use one programming language; supported runtime packaging still needs clear constraints.
- No assumption that every support report represents an infrastructure fault.
- No unrestricted autonomous administration by an AI model.

## Distribution and sustainability

The core capabilities must be available for self-hosting under a selected OSI-approved license, using open-source dependencies and open interfaces as defined in the [project policy](open-source-and-standards.md). Optional paid offerings may provide managed hosting, support, and operational convenience; essential capabilities must not depend on them. The specific license, governance model, and commercial packaging remain open decisions.

## Success signals

Evaluate the product through observed outcomes: successful first deployments, verified restores, recovery from interrupted operations, understandable failure reports, measured migration interruption, and reduced operator effort. Performance and reliability targets should follow representative prototypes and testing rather than unsupported estimates.
