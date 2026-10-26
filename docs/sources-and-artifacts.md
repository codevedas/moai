# Replaceable source and artifact providers

**Status: required product direction; interfaces are proposed.** Git is one possible source adapter. Neither Git nor a particular repository hosting service may be required by Moai's core deployment model.

## Boundaries

Keep source acquisition, building, artifact storage, and deployment separate:

`source provider → immutable source snapshot → build adapter → immutable artifact → deployment adapter`

A prebuilt artifact can enter directly at the artifact step. This remains the first-release path, so a deployment does not require a repository, branch, commit, or build server.

Source integrations may support Git, another version-control system, a checksummed source archive, or a future source system. An integration with a repository hosting service is distinct from the version-control implementation: hosting features such as review requests, checks, and webhooks are optional capabilities.

## Proposed source contract

A source adapter should declare its supported operations and return:

- Provider and source identities, independent of display names or URLs.
- An opaque provider revision and an immutable snapshot reference with a verified content digest.
- Source provenance, retrieval time, and the resolved version of any mutable selector.
- Optional metadata such as authorship, change descriptions, branches, or review links when available.
- Credential references and scoped access requirements, without embedding secrets in exported records.

Do not require a Git-shaped revision identifier or branch model in shared schemas. Resolve mutable selectors to pinned snapshots before building. Document provider-specific snapshot exclusions and normalization so verification is meaningful.

Optional operations such as watching changes, comparing revisions, or reporting build results need explicit capabilities. Unsupported features remain unavailable rather than being silently approximated.

## Build and artifact contracts

A build adapter consumes the pinned snapshot plus a versioned build specification. It records inputs, builder identity, output digest, and provenance. Source builds require an isolation and secret-handling design because build instructions are untrusted executable content.

The artifact interface records type, digest, location, provenance, and verification results. For the initial OCI workload adapter, deployment uses a pinned OCI image digest. Other artifact types require runtime adapters that explicitly support them.

A release references the immutable artifact, configuration revision, and optional source/build provenance. Rollback uses a retained compatible artifact; it must not depend on rebuilding an old branch or retaining access to a source provider. Retention policies must preserve artifacts required for supported recovery workflows.

## Events and switching providers

Normalize authenticated provider notifications into scoped Moai events. Verify producer identity, resolve the immutable source revision, and deduplicate before triggering a build. Webhooks and polling are adapter capabilities; neither is a universal prerequisite.

Changing a source or hosting provider must not change the core release schema or invalidate existing artifact-based deployments. It may require credential changes, selector remapping, and new subscriptions. Transferring repository history or reviews is a separate migration capability and is not implied by adapter replacement.

## Acceptance evidence

First prove deployment from a pinned artifact without Git installed or repository credentials. Before declaring source workflows provider-independent, run the same build contract through a Git adapter and a non-Git source adapter such as a verified archive. Verify source pinning, invalid digests, lost provider access, duplicate notifications, and rollback using retained artifacts while the source provider is unavailable.

Moai contributors may use Git for Moai's own development. That choice must not impose Git on operators' deployment workflows.
