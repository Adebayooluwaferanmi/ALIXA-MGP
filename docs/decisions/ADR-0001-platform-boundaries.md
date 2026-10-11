# ADR-0001 — Stable platform vs versioned creative execution

Status: **Accepted as planning baseline** · 2026-10-11

## Context
The original ALMS marketing pipeline required committing each creative edit to GitHub and redeploying the Docker renderer, followed by manual authenticated API calls and downloads. The media platform must accept custom code, assets and browser captures without operational friction, while keeping cloud cost and security bounded.

## Decision
- Studio web/API services are stable and deploy only when platform code changes.
- Creative projects are versioned manifests and immutable code/asset snapshots stored separately.
- Trusted templates use dynamic Remotion inputProps and cached bundles.
- Custom Remotion code is compiled only within isolated, unprivileged, limited workers; never in Studio API or live trusted renderer.
- Import Google Drive/GitHub resources into immutable internal snapshots (not live mounts).
- Render through queues; separate browser capture/audio tasks, use S3-compatible object storage across services.
- Reuse existing Ordabase ALMS renderer temporarily as trusted adapter, not as untrusted arbitrary-code runner.

## Consequences
Benefits: no deploy per creative revision, reusable templates, traceability, recovery, better cost accounting. Costs: queue, object store, sandbox complexity, version management. Alternative (commit every new video to Git) rejected for frequent revisions; alternative (run uploaded code in API) rejected for security.

## Follow-up ADRs
ADR-0002 sandbox implementation; ADR-0003 storage provider; ADR-0004 TTS model and commercial rights; ADR-0005 identity and workspace roles; ADR-0006 Render project and service topology. Record actual decisions rather than asserting unapproved providers.
