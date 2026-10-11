# System architecture · baseline

## Design tenets
1. Build the **studio once**; revise **videos as versioned data and code snapshots**.
2. Remotion composes/renders video and audio; external engines synthesize speech/music.
3. Separate control plane (web/API/DB/queue) from untrusted execution plane (build/capture/render).
4. Imported GitHub/Drive content is pinned, verified and copied to managed storage; never render against mutable branch HEAD or live Drive.
5. Cost limits enforced before work admission, not after overuse.

## Logical components
- `apps/web`: Next.js, Monaco, project timeline, Remotion Player, local-tab MediaRecorder UI, asset library, job monitoring.
- `apps/api`: auth, tenants, projects, immutable manifests, source import, asset metadata, approval workflow, budget checks.
- `packages/contracts`: JSON Schema v1 + typed DTOs/OpenAPI, API/event versioning.
- `workers/build`: isolated bundler for trusted Remotion code; no ambient cloud tokens; allowlisted and pinned deps; output SHA256-addressed bundle.
- `workers/render`: Remotion renderer; consumes signed bundle/assets + manifest; limited parallelism; outputs in object storage; emits structured job events.
- `workers/capture`: isolated Chromium with network egress policy and per-user session; screenshot and session video.
- `workers/audio`: TTS provider adapters, audio duration alignment and FFmpeg; music library first.
- Postgres: authoritative state; Redis: job scheduling/locks; S3-compatible bucket: source snapshots, bundles, captures, outputs.

## Production data flow
1. User saves project draft; API creates version snapshot with manifest and SHA-256 content hashes.
2. Validate manifest, composition capabilities and all asset references; reject invalid or unauthorized inputs.
3. Resolve imported assets (GitHub commit SHA/Drive revision) into immutable internal object keys and scan.
4. If trusted bundle cache missing, enqueue isolated build; cache by code hash + package-lock hash + runtime + engine version.
5. Generate/approve narration; lock audio asset versions and rights metadata.
6. Queue preview/final render under immutable job ID and idempotency key.
7. Worker leases job; emits heartbeats, progress events and capped logs; output uploads are atomic and checksummed.
8. FFprobe technical QA (fps/size/duration/audio track), thumbnail/contact sheet, report warnings.
9. Reviewer approves or requests new draft; signed download/export to Drive.

## Interfaces (representative; versioned)
`POST /api/v1/projects`, `GET /api/v1/projects`, `POST /api/v1/projects/{id}/versions`,
`POST /api/v1/imports/github`, `POST /api/v1/imports/drive`, `POST /api/v1/assets/uploads`,
`POST /api/v1/builds`, `POST /api/v1/previews`, `POST /api/v1/render-jobs`,
`GET /api/v1/render-jobs/{id}`, `GET /api/v1/render-jobs/{id}/events`,
`POST /api/v1/voice-clips`, `POST /api/v1/capture-sessions`, `POST /api/v1/approvals`.
All writes authenticated; idempotency keys for job creation; SSE or polling for progress.

## Job states
`draft -> validated -> queued -> building? -> ready -> rendering -> qa -> completed`; terminal `failed|cancelled`. Retry creates attempt with lineage and fixed input snapshot. Leases/heartbeats prevent concurrent double processing. Cancellations kill child process trees.

## Data model (minimum)
Workspace, Membership, Project, ProjectVersion, SourceSnapshot, Asset, AssetVersion, LicenseGrant, Manifest, Composition, BuildArtifact, CaptureSession, VoiceProfile, AudioClip, RenderJob, JobAttempt, Output, Review, AuditEvent, BudgetPolicy.

## Key security controls
**Custom JavaScript is arbitrary code.** No shared Node process, unguarded child_process, privileged mounted host disk or shared credentials. Ephemeral container/microVM ideally no root, no Docker socket, read-only root, seccomp, PID/memory/CPU/time quotas, constrained outbound network and per-job workspace. Runtime supply-chain pinning and package allowlist; isolate Chromium from metadata/private IPs (SSRF/DNS rebinding protections). No cross-tenant cookies. Local tab capture requires browser permission. redact PHI before sharing or publishing. Never auto-voice-clone someone without consent.

## Integration with existing Ordabase renderer
Current `alms-video-renderer` uses one active in-memory job, hard-coded composition IDs and fixed 90s validation. Treat it as a migration **adapter only**, not a general untrusted code executor. Add manifest-aware rendering in a separate service or isolated job runner; retain existing service and credentials until migration passes regression. Existing paid resources are not automatically duplicated.

## Data retention/recovery
Object lifecycle policies for temporary frame files and abandoned bundles; configurable outputs retention. Back up Postgres, pin object-store revisions, periodically test recovery and failed-render cleanup. No storage provider assumed until budget/availability approval.

## Decision log
See ADR-0001. Open ADRs: execution isolation provider, bucket/provider, IdP, actual build-runner and GPU/TTS strategy. Never treat open ADRs as implemented.
