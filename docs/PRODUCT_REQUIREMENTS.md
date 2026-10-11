# Product requirements (PRD) · v1.0

## Vision and positioning
ALIXA-MGP is a self-hostable, code-first media studio for repeatable marketing videos, product walkthroughs, training content and explainers. First pilot: ALMS. ALMS is a separate product; the parent brand is ALIXA.

## Problems
- Every creative change in the current ALMS Remotion film requires code changes and app redeployment.
- Screenshots and recordings are collected outside the production process and manually moved.
- Rendering requires PowerShell and direct API tokens; jobs and outputs lack a team-facing library.
- Narration/music need separate generation and approval.
- Cost, tenant isolation and untrusted code risks are not centrally governed.

## Personas
Producer (projects/storyboards), motion developer (Remotion source), designer (assets/captures), narrator/audio editor, reviewer/approver, platform admin/operator.

## Scope
### Must-have for usable MVP (P1)
- Authentication, project/workspace RBAC, project and version lifecycle.
- Manifest schema, template registry and code-project snapshot ingestion (code mode initially restricted to trusted authors).
- Asset upload, hash, MIME validation, folder import snapshots, provenance, immutable references.
- Source editing with Monaco, build/preview errors and immutable revisions.
- Queue jobs, status/progress/logs, cancellation, retry, idempotency, storage/download.
- 16:9 and 9:16 at 30fps; dynamic duration, not hard-coded 90s.
- At least one existing Remotion composition migrated as manifest-driven template.
- Frame preview and full render with stable application deployment.
### Browser Capture (P2)
- Local permissioned tab capture, screenshot/video, pause/stop, cropping, redaction and saving into project.
- Remote controlled Chromium via authenticated isolated sessions and Playwright; 1 session initially; no secrets embedded in recording.
- Capture-to-scene and provenance.
### Audio (P3)
- Pluggable self-hosted TTS with narrator profile/version and pronunciation overrides; scene-level approval/regenerate.
- Licensed soundtrack library; optional generative-music adapter after model/output license review.
- FFmpeg mixing, fades, ducking, loudness and playback QA.
### Advanced (P4)
- Trusted custom code build in isolated ephemeral executor, strict dependency/egress policies, deterministic bundle cache.
- Timeline editing, review annotations, diff/compare exports and quotas.

## Excluded from MVP
Open internet execution of arbitrary third-party npm packages, unreviewed voice cloning, unlimited free GPU music generation, public multi-tenant browser recording, automatic publication of generated media, full NLE editing parity.

## End-to-end acceptance scenario
Given a team member with Producer permission and an approved ALMS project: open Studio; create v2 from a trusted template; import a Google Drive resource folder as immutable snapshot; capture an authorized demo tab; put capture in scene 3; update copy/narration; render 16:9 and 9:16; review outputs; download files. **No new Git commit, web service deploy, manual token or PowerShell command.** Changes to engine code remain normal deployments.

## Nonfunctional expectations
- Versioned and immutable render input hashes; no mutable source drift.
- Authenticated asset delivery with signed short-lived URLs; encryption in transit and at rest.
- Audit records for project/source imports, capture access, model voice use and publication approval.
- All jobs recover or cleanly fail on worker restart; no permanent in-memory sole source of truth.
- Explicit budgets for CPU/memory, render time, output retention and storage.
- Accessible keyboard navigation; responsive producer UI; usable on Nigerian connections.
- Metrics: success rate, render queue latency, actual compute seconds, memory peak, asset bytes, output egress, preview cache hit rate.

## Outcomes and measurement
- Zero production redeployments for standard video edits.
- >=95% successful render jobs in pilot after excluding invalid projects (target to validate).
- 100% jobs trace back to pinned manifest, code revision, assets and audio versions.
- Reproducible output metadata and deterministic compilation where possible; document nondeterministic models.
- Pilot user can create and download a complete ALMS video without engineer assistance.

## Unresolved product decisions
Commercial licensing, expected number of team users, initial production budget, object-store provider and TTS voice license/reference ownership. Each requires explicit owner approval before production expansion.
