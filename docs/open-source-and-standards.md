# Open source and open standards

**Status: required project policy.** Implementation and verification are pending. This policy constrains future technology choices; it does not claim that an implementation already conforms to any specification.

## Open-source foundation

Moai — Mother of All Infra — must provide a complete self-hostable platform using open-source software. This includes the dashboard, control plane, execution components, deployment and migration engines, automation, and first-party integrations. Essential capabilities must not require a proprietary edition, license server, vendor account, or paid hosted control plane.

Use the [Open Source Initiative's Open Source Definition](https://opensource.org/osd) as the software licensing baseline. Source availability alone is insufficient. Select an OSI-approved license for Moai before public code release; the exact license remains undecided.

Required build tools and runtime dependencies must be available under OSI-approved licenses. Check the exact dependency version, distribution, bundled components, and transitive dependencies rather than assuming that a project's name establishes its license. Review compatibility with Moai's selected license and preserve required notices and source distribution obligations.

Optional integrations may connect to proprietary clouds, external services, or hosted models chosen by the operator. They must be clearly identified, disabled unless configured, and replaceable without disabling the core platform. This policy does not restrict the license of applications that users deploy with Moai.

## Open interfaces and portability

Prefer published, vendor-neutral specifications with implementable contracts and independent tooling. Use existing specifications where they fit before inventing a format or protocol. A product-specific API may be supported through an adapter, but must not become the sole representation of Moai's resource model.

Publish versioned schemas and extension contracts. Document supported versions, optional capabilities, deviations, and compatibility tests. Operators must be able to export configuration and operational records in documented formats and recover backups without a mandatory Moai-hosted service. Handle secret export separately through an explicit protected workflow.

## Standards baseline

| Area | Required direction when the capability is implemented | Primary reference |
| --- | --- | --- |
| Container artifacts and execution | Use OCI image, distribution, and runtime specifications at the applicable boundaries; select an open-source implementation. | [Open Container Initiative](https://opencontainers.org/) |
| HTTP API descriptions | Publish an OpenAPI description for supported HTTP APIs and validate implementation behavior against it. Other transports need equivalent published contracts. | [OpenAPI Initiative](https://www.openapis.org/) |
| Agent integration | Use MCP for exposing context/tools and connecting approved external MCP servers; document supported revisions and capabilities. | [Model Context Protocol](https://modelcontextprotocol.io/specification/2025-11-25) |
| Event exchange | Use CloudEvents envelopes with versioned Moai payload schemas; specify delivery guarantees separately. | [CloudEvents](https://cloudevents.io/) |
| Observability | Use OpenTelemetry instrumentation and OTLP export where applicable, with a configurable collector/backend. | [OpenTelemetry](https://opentelemetry.io/docs/what-is-opentelemetry/) |
| Dependency inventory | Use SPDX license expressions and publish an SPDX software bill of materials for release artifacts, including bundled components. | [SPDX](https://spdx.dev/learn/overview/) |

Select exact specification versions during implementation and record them in the compatibility matrix. Adopting these interfaces does not prescribe a programming language or require every possible integration in the first release.

## AI components

AI assistance must remain optional for ordinary deployment and operations. The model interface must support a self-hosted path when AI features are introduced; a commercial model API must never be the only route to those features.

Review inference software and model artifacts separately. Accessible weights do not by themselves establish that a model is open source. Record model licenses, restrictions, provenance, and hardware requirements explicitly; do not describe a restricted model as satisfying the open-source baseline. The exact local model and its acceptance criteria remain to be selected and verified.

## Dependency and release checks

Before introducing a dependency, record its purpose, exact version, source, license expression, redistribution obligations, and whether it is required or optional. Prefer a maintained open-source implementation that satisfies the needed contract. Record selection rationale for major infrastructure components.

Before publishing an executable release:

1. Select and include Moai's license and required dependency notices.
2. Inventory direct, transitive, and bundled dependencies and resolve unknown or incompatible licensing.
3. Publish an SPDX bill of materials tied to the actual release artifacts.
4. Demonstrate the documented self-hosted build and runtime path without proprietary services or paid accounts. Document any network access needed to fetch public dependencies.
5. Validate applicable API and adapter contracts against the declared specification versions.
6. Test export and recovery using the documented formats and open-source tooling.

Automate these checks in CI once code and release artifacts exist. Passing a license scanner alone does not establish compatibility; unresolved findings block release until reviewed. Changes to this policy require an explicit documented project decision.
