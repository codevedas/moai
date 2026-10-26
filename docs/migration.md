# Migration and recovery model

**Status: design requirements and proposed workflow.** Migration is a later milestone than the initial deployment release.

## Scope of a supported migration

A migration is a coordinated move of an application and its declared dependencies between explicitly supported source and destination configurations. Support must be stated per runtime, service version, data transfer method, architecture, and storage arrangement.

The initial proposal is a controlled maintenance-window move between two compatible VPS hosts. It should favor a verifiable data transfer and clear recovery procedure. Replication-based lower-downtime methods can be added as separately tested capabilities.

External services remain in place unless their adapters explicitly support and authorize moving them. The plan must verify destination connectivity to those services.

## Preflight and plan

Before execution, discover and validate:

- Application images, configuration revisions, secrets, persistent volumes, and dependencies.
- Database versions, extensions, data size, backup compatibility, and restoration method.
- Whether Redis/Valkey holds disposable cache, durable state, queue data, or a mixture.
- Workers, scheduled jobs, queue ownership, active work, and external side effects.
- Destination capacity, architecture, network paths, credentials, and storage.
- Routing, certificates, DNS behavior, outbound addresses, and external allowlists.
- Shared dependencies, unsupported resources, and manual prerequisites.

The resulting plan declares the transfer method, expected interruption, data-loss tolerance, verification criteria, source retention period, and recovery boundaries. Estimates must identify their assumptions; a rehearsal can improve them.

## Proposed operation states

`planned → preflight → preparing → quiescing → transferring → verifying → cutting_over → observing → completed`

Every active state can enter `failed` or `needs_operator`. Recovery is a recorded operation with its own preconditions. A cancellation request is honored only at a safe checkpoint and must report what has already changed.

## Execution sequence

1. Prepare the destination and verify required resources and external connections.
2. Capture a recoverable backup and confirm that the restore method is usable.
3. Enter maintenance mode or otherwise prevent new writes as defined by the adapter.
4. Pause scheduled-job ownership and drain or stop workers according to application policy.
5. Fence source writers so an isolated or restarted source cannot resume writing unexpectedly.
6. Transfer the final consistent data and restore it on the destination.
7. Verify data and application health before allowing destination writes or production traffic.
8. Change routing and transfer worker and scheduler ownership through recorded steps.
9. Observe health and data consistency for the configured period.
10. Retain the source according to policy; cleanup is a separate, explicitly authorized operation.

Adapters may refine this sequence, but they must preserve its stated consistency and recovery guarantees.

## Jobs, queues, and side effects

Pausing a scheduler does not cancel work already dispatched. A migration needs an explicit policy for in-flight tasks, queued tasks, missed schedules, and retries.

The initial proposal is one active scheduling owner per job, backed by a durable ownership mechanism and a way to fence stale owners. Applications still need idempotency or deduplication for externally visible effects such as charging a customer or sending a notification. The platform cannot promise exactly-once effects for arbitrary application code.

Queue migration support must be defined for the particular queue implementation. Copying a Redis or Valkey dataset alone does not prove safe worker handover.

## Rollback boundaries

Before the destination accepts new writes, recovery may restore traffic and ownership to a verified source. After destination writes begin, redirecting traffic back to an old source can discard new data or create divergence.

The plan must mark this boundary. Beyond it, recovery may require reverse synchronization, a new backup/restore, or operator intervention. Application release rollback and database rollback are separate capabilities; an old application image may not understand a newly changed schema.

DNS updates alone do not fence the source because clients and intermediaries may retain old destinations. The supported method must account for continued source traffic.

## Evidence required before support is claimed

Test clean migration, failed transfer, failed restore, interrupted control-plane execution, network partition, insufficient storage, failed cutover, stale source restart, and destination failure after writes begin. Include data validation and checks for duplicate or missed background work.

Record the actual interruption window, recovery time, and any data loss in each scenario. A successful happy-path demo is insufficient to claim production migration support.
