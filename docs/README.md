# Documentation

These documents capture the intended product and an initial design proposal. They do not describe implemented software.

## Reading order

| Document | Purpose |
| --- | --- |
| [Stage 0 completion](stage-0.md) | Requirements traceability, definition exit checks, and the implementation handoff. |
| [Product baseline decision](decisions/0001-product-baseline.md) | Accepted product constraints and intentionally deferred implementation choices. |
| [Vision](vision.md) | The user problem, desired experience, and product boundaries. |
| [First release](first-release.md) | A small, testable initial scope and its acceptance criteria. |
| [Architecture](architecture.md) | Proposed components, resource model, and extension interfaces. |
| [Sources and artifacts](sources-and-artifacts.md) | Replaceable version-control, source, build, and artifact integrations. |
| [Migration](migration.md) | Data movement, cutover, job coordination, and recovery expectations. |
| [AI operations](ai-operations.md) | How reports become investigations and controlled changes. |
| [Agents, events, and analytics](agents-events-and-analytics.md) | MCP interfaces, configurable hooks and alerts, and shared analytics. |
| [Engineering](engineering.md) | Working standards, stack selection criteria, and unresolved decisions. |
| [Open source and standards](open-source-and-standards.md) | Required licensing, portability, interface, and release policies. |
| [Roadmap](roadmap.md) | Delivery stages ordered by prerequisites and evidence. |

## Document conventions

- **Requirement** expresses intended product behavior derived from the project discussion.
- **Proposal** is an initial design choice that still needs validation.
- **Open decision** has not been settled and must not be treated as an implementation commitment.
- Acceptance criteria describe evidence needed before a capability can be called complete.

The project is named **Moai — Mother of All Infra**. Documents should change as prototypes and operator experience reveal better choices. Record consequential decisions with their rationale and revisit conditions rather than silently treating an early suggestion as settled.
