# ADR-0001 — Separate creative project lifecycle from platform runtime

**Status:** Accepted design principle · **Date:** 2026-10-11

## Context

Programmable media projects consist of executable compositions, input data, captured footage, generated audio and processing metadata. When the infrastructure deployment and creative project share the same lifecycle, iterations require infrastructure-level operations that are unnecessary for changes confined to media content. Executing arbitrary compositions in a privileged application process also creates unacceptable security and resource-isolation risks.

## Decision

1. Implement ALIXA-MGP as a **Linux-native, self-hostable platform** with independently versioned application runtime and creative project snapshots.
2. Store creative source, dependencies, manifests and assets as content-addressed or immutable objects; record exact production provenance.
3. Compile approved Remotion compositions into cached bundles using isolated and restricted Linux build workers.
4. Keep media rendering, remote browser capture, and audio synthesis in separate bounded execution contexts. Do not expose application credentials to creative code.
5. Place build artifacts, input media and outputs in provider-neutral object storage; maintain authoritative lifecycle metadata in PostgreSQL.
6. Permit GitHub and Google Drive as optional import/export integrations, never core runtime dependencies.
7. Select hosting independently from the product architecture. Linux VMs/hosts/containers must be supported without reliance on any branded hosting account.
8. Use data-only revisions without rebuilding when possible, but rebuild cached bundles when custom source or locked dependencies change.

## Rejected alternatives

- **Single privileged process for user-supplied React code:** violates isolation and resource governance.
- **Mandatory application redeployment per composition change:** conflates creative operations with product releases.
- **Live rendering against mutable remote files:** compromises provenance and reliable reproduction.
- **Provider-specific infrastructure coupling:** limits portability and future deployment choices.

## Consequences

More infrastructure components (queue, storage, isolated workers), but clear ownership, bounded resource usage, repeatable outputs, safer extensibility and stable platform releases.

## Follow-up ADRs

Sandbox/namespace/VM boundaries; media storage choice; worker scheduling; authentication and workspace model; speech engine and licensing; browser capture egress policy; Linux deployment topology.
