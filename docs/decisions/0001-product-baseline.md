# 0001 — Product baseline

**Status:** accepted product requirements; implementation choices remain pending.

## Context

Moai — Mother of All Infra — must support a developer starting with one server and evolving applications, services, and infrastructure through a coherent, extensible platform. The core experience includes deployment, recovery, migration, observability, analytics, and configurable AI assistance.

## Decision

1. Provide a complete self-hosted path using open-source components and published interfaces, following the [open-source policy](../open-source-and-standards.md).
2. Begin with a single supported host and prebuilt OCI images. Add providers and service implementations through capability-based adapters.
3. Keep source retrieval, builds, artifact storage, and deployment independent. Git and repository hosting are optional integrations; releases reference immutable artifacts.
4. Make model providers, endpoints, models, task routing, budgets, and permitted fallbacks configurable. Ordinary operation must remain available without model inference.
5. Expose structured context and capabilities through MCP and ordinary APIs. Use the same authorization and execution policies for human and agent interfaces.
6. Make events, alerts, analytics, and notification integrations cross-cutting capabilities with scoped access and replaceable implementations.
7. Record changes as inspectable, durable operations with explicit verification and recovery behavior. AI output cannot bypass policy or grant itself authority.
8. Treat stateful migration and job coordination as explicit workflows with tested compatibility and downtime limits.
9. Deliver useful stages with acceptance evidence; defer advanced integrations without weakening the contracts that allow them.

## Alternatives considered

A fixed stack tightly coupled to one repository host, cloud, or model would simplify early integration but conflict with provider independence. Implementing every provider and plugin before the first release would delay validation of deployment and recovery. The chosen approach establishes narrow contracts, implements a small supported path, and expands using operational evidence.

## Consequences

Adapter support must be capability-specific and tested. Replacing a component does not imply automatic conversion of its data or history. The initial release has fewer integrations than the long-term vision, but does not require proprietary services, Git, or an LLM to deploy an application.

The implementation language, persistence, transport, authentication mechanism, exact license, and specification versions are not selected by this decision. They remain in the [engineering register](../engineering.md#open-decision-register), with explicit deadlines before dependent work or release.

## Revisit conditions

Prototype findings may change component boundaries or delivery order when the product requirements remain satisfied. Changes to the accepted requirements require an explicit superseding decision with rationale. Update the requirement map and affected acceptance criteria with any such decision.
