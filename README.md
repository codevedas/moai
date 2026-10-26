# Moai — Mother of All Infra

**An open, extensible platform for deploying, operating, and evolving application infrastructure.**

Moai aims to bring applications, infrastructure services, deployment workflows, observability, migration, and AI-assisted operations into one coherent system. Start on a single VPS, connect the services you already use, and change your infrastructure as your applications grow.

> **Status: Stage 0 complete — product definition.** This repository currently contains project documentation only. There is no installable release, implemented API, or production-ready runtime. Stage 1 begins with the implementation stack and core technical decisions. The license remains undecided. See the [Stage 0 completion record](docs/stage-0.md).

## The vision

A developer should be able to describe what an application needs, deploy it, understand its health, and safely change the infrastructure supporting it.

For example, a project might begin with an application and PostgreSQL on one VPS. As demand grows, the platform could identify a bottleneck, recommend a cache or worker, explain the evidence and tradeoffs, and prepare an approved change. Later, it could coordinate moving the application and its state to another server, accounting for traffic, background workers, scheduled jobs, and recovery.

The same platform should support multiple projects with different languages and operational requirements. Its services, providers, AI models, and dashboard should be extensible through documented interfaces.

## Intended capabilities

| Area | Intended behavior |
| --- | --- |
| Deployment | Manage applications, environments, releases, workers, and scheduled jobs; expand to configurable rollout strategies. |
| Source and build providers | Deploy prebuilt artifacts or use replaceable source/build adapters; Git and repository hosting services are optional integrations. |
| Infrastructure | Begin on one VPS or cloud VM; add providers and execution backends through adapters. |
| Services | Provision supported services or connect existing databases, Redis/Valkey, and other dependencies. |
| Operations | Bring health checks, logs, metrics, backups, restore workflows, and incident history together. |
| Migration | Plan and coordinate supported moves across servers, with declared downtime and recovery limits. |
| AI assistance | Investigate monitoring alerts and support reports, recommend changes, and execute permitted workflows under policy. |
| Configurable LLMs | Choose local or hosted providers and models per project or task, with configurable endpoints, limits, data policies, and opt-in fallbacks. |
| LLM and MCP interfaces | Expose structured context and scoped operations to agents; connect approved external MCP tools. |
| Events and alerts | Attach rules and hooks across resources and workflows; ingest custom events and route notifications through adapters. |
| Analytics | Share operational, cost, and optional application analytics across dashboards, alerts, APIs, and agents. |
| Extensibility | Support service, provider, observability, model, workflow, and UI extensions. |
| Customization | Allow configurable dashboard layouts, widgets, presets, and integrations. |

These are design goals, not a list of implemented features. Connecting a service does not imply that Moai can administer or migrate it. Interchangeable integrations must declare their capabilities and compatibility limits.

## First useful release

The proposed first release focuses on one operator managing containerized applications on one Linux VPS:

- Deploy an application from a supplied container image.
- Manage PostgreSQL, optional Valkey, workers, and scheduled jobs using a supported preset.
- Track configuration, releases, health, and operation history.
- Back up and restore a supported PostgreSQL deployment.
- Recover from a failed application rollout where data compatibility allows it.
- Attach an existing service through an explicit connection configuration.

A controlled migration between two supported VPS hosts follows once deployment, restore, and recovery are dependable. AI diagnosis, additional providers, canary deployments, and a broader plugin ecosystem build on that foundation.

See [release scope](docs/first-release.md) and the [roadmap](docs/roadmap.md) for acceptance criteria and deferred work.

## Design principles

1. **Self-hosting first.** The core should be useful on a single server without a mandatory hosted service.
2. **Open source and open standards.** The platform and required dependencies must use open-source licenses, with published interfaces and a complete self-hosted path. Essential capabilities must not require proprietary services. Optional managed hosting and support may fund development. See the [project policy](docs/open-source-and-standards.md).
3. **Explicit capabilities.** Integrations describe what they can observe, manage, back up, restore, and migrate.
4. **Recoverable operations.** Changes have recorded plans, progress, verification, and failure handling.
5. **Controlled automation.** Policy and operator approval govern consequential changes; AI uses the same operation interfaces as human operators.
6. **Stable contracts.** Technical identifiers and public interfaces should evolve independently of branding.

## Documentation

- [Documentation index](docs/README.md)
- [Stage 0 requirements and completion record](docs/stage-0.md)
- [Vision and product requirements](docs/vision.md)
- [Proposed architecture and extension contracts](docs/architecture.md)
- [Replaceable source and artifact providers](docs/sources-and-artifacts.md)
- [First release and acceptance criteria](docs/first-release.md)
- [Migration and recovery model](docs/migration.md)
- [AI assistance and operation policies](docs/ai-operations.md)
- [LLM interfaces, MCP, events, alerts, and analytics](docs/agents-events-and-analytics.md)
- [Engineering standards and open decisions](docs/engineering.md)
- [Open-source and open-standards policy](docs/open-source-and-standards.md)
- [Roadmap](docs/roadmap.md)
- [Contributing](CONTRIBUTING.md)

## Getting started

Start with the vision and first-release documents, then review the architecture and engineering decisions. Installation instructions will be added when an executable prototype exists. No installation commands are available yet.

## Name, website, and license

Moai stands for **Mother of All Infra**. The project repository is [codevedas/moai](https://github.com/codevedas/moai). A subdomain can serve as the initial website; a dedicated domain is not required to begin development. No project website has been selected yet.

The project is intended to be open source, but **no license has been selected or granted by this repository yet**. License selection is a prerequisite for the initial public code release. References to open source describe the intended distribution model.
