# AI assistance and operational policy

**Status: proposed behavior.** AI assistance follows the deployment and recovery foundations.

LLM-friendly resource contracts and MCP interfaces begin with the foundation, before autonomous diagnosis or remediation. See [agents, events, and analytics](agents-events-and-analytics.md) for inbound/outbound MCP, alert hooks, and shared analytics requirements. Agent tools must use the same policy and workflow engine as other interfaces.

## Investigation flow

An alert, support ticket, or support email can provide a starting signal. The platform associates it with a project, gathers permitted evidence, and produces a diagnosis with supporting observations and uncertainty.

The intended flow is:

`report → project association → evidence collection → hypotheses → proposed plan → policy/approval → execution → verification`

Reports may describe application bugs, user mistakes, or external outages. The system must permit an inconclusive result and a recommendation for human investigation.

## Evidence and model access

Use bounded, relevant telemetry and deployment history. Record which observations support a recommendation and when those observations were collected. Redact credentials and avoid sending unnecessary customer or support data to model providers.

Logs, tickets, emails, and retrieved content are untrusted evidence. Instructions embedded in them must not change platform permissions or cause actions. Model output is also an untrusted proposal until validated against resource schemas, capabilities, and policy.

Model providers should be replaceable through an adapter contract. A local model or an external model may have different data handling and resource requirements, which the operator must be able to configure.

## Configurable LLMs

**Requirement:** the LLM is an operator-configurable dependency. No provider or model is hard-coded as the only supported choice. Support multiple named model profiles and a self-hosted path, subject to the [open-source policy](open-source-and-standards.md).

Each profile should configure:

- Provider adapter, model identifier/version, local or hosted endpoint, and credential reference.
- Supported capabilities, including structured output, tool calling, streaming, and context limits; validate task requirements before execution.
- Supported generation settings, input/output token limits, timeouts, concurrency, and retry limits. Reject unsupported options rather than silently ignoring them.
- Usage limits and spending budgets where applicable, with explicit behavior when limits are reached. Report estimated costs separately from actual provider billing.
- Allowed data categories, redaction, retention settings under Moai's control, and permitted external destinations. Do not imply Moai can enforce a provider's internal retention behavior.
- Explicit fallback profiles, failure conditions, and whether an operator must approve escalation.

Allow platform defaults with project, environment, and task/workflow overrides. For example, an operator could use a small local model for alert summaries and a different approved model for incident investigation. Resolve the effective configuration visibly; narrower scopes may not relax administrator-enforced data, spending, or permission limits.

Fallback is opt-in and must preserve task capability requirements and data policy. A failed local model must not silently send private context to a hosted provider. If no permitted fallback is available, pause the AI task with an actionable status while core operations continue. Retrying inference must not replay already-executed tool actions.

Version profiles and record the effective profile revision, model identity, and adapter version for each run. Apply profile changes to new runs; changing an in-progress run requires an explicit recorded transition. Revalidate resulting plans through the same policy and approval process regardless of which model produced them.

MCP configuration and model configuration are separate: MCP supplies authorized context and tools, while the selected model profile controls inference. An external MCP client may choose its own model; Moai still enforces all permissions on its server. Operators must be able to disable Moai-managed inference without disabling MCP access, deployment, alerts, or analytics.

## Proposed automation levels

| Level | Permitted behavior |
| --- | --- |
| Observe | Read authorized evidence and summarize it. |
| Recommend | Produce a diagnosis and inspectable change plan. |
| Execute with approval | Execute a specific approved plan through the workflow engine. |
| Policy-limited automation | Execute pre-authorized operation types within explicit resource and impact limits. |

Start with observation and recommendations. Approval-based execution requires reliable adapters and recovery behavior. Broader automation requires evidence that the specific operation class is safe under its defined constraints.

## Authority and execution

The model receives no general-purpose authority to administer hosts. All changes go through typed operations with authorization, preconditions, plan revision checks, and audit records.

Policy should consider resource scope, operation type, data impact, expected downtime, cost implications, and recovery options. A generated confidence score is not permission to act. Material changes to the plan or observed infrastructure require a new policy evaluation and, where applicable, renewed approval.

Support ingestion and sending replies are separate capabilities. Reading a report does not authorize sending a response to its author.

## Example

A support report says an application is slow. The investigation finds increased request latency after a release and repeated database queries. It can propose an application investigation, a release rollback if compatible, or a measured caching experiment. Adding Redis/Valkey may require application code and cache invalidation design; provisioning the service alone does not resolve that requirement.

## Verification

Evaluate whether investigations identify the affected resource, cite relevant evidence, handle missing data, resist instructions embedded in reports, respect project boundaries, and abstain when uncertain. For execution, test policy rejection, expired approvals, stale plans, failed actions, and post-change verification.

Test profile inheritance, model switching, unsupported capabilities/settings, invalid credentials, exhausted budgets, provider outages, permitted and prohibited fallbacks, and disabling inference. Verify that configuration changes cannot widen tool authority or bypass data policy and that retries cannot duplicate executed actions.

Record model and adapter versions, the evidence references, policy decision, approved plan, execution events, and verification outcome while applying configured retention and data minimization rules.
