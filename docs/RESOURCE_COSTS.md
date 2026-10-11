# Resource strategy and cost controls

## Baseline
Existing Ordabase `alms-video-renderer` is a paid Render Docker service with 2 vCPU, 4 GB RAM and attached disk. Do **not** repurpose its privileged process to execute arbitrary source; preserve it as a legacy/trusted adapter during migration. Reuse its capacity only after benchmarking.

## Defaults (proposals, not observed limits)
- Max full render concurrency: 1 on current worker.
- Default preview: one frame or <=10 sec low-resolution clip; no automatic full render on every keystroke.
- Browser sessions: one active remote session, ~10-minute idle timeout, max recording duration configurable.
- Code builds: content-hash caching; limited CPU, RAM, wall-clock, IO, PID and egress, isolated execution.
- TTS: cache audio by script/voice/model hash, only regenerate changed clips.
- Media: thumbnails and proxy previews; originals and final versions on S3-compatible storage with retention.
- Exports: signed short-lived download URLs and lifecycle expiration; no unnecessary file duplication.

## Scheduling
Priority queues: interactive frame preview > short scene preview > full render > batch export. Admission control rejects jobs over quota. Fair-use/workspace budgets and manual approval thresholds. No guaranteed dollar estimate until measured compute seconds, storage, egress and model costs are available.

## Metrics
Record queue delay, compile time, worker CPU-seconds, peak RSS, render time by resolution, browser minutes, audio/model inference duration, byte growth and egress, failed/retried jobs and cache hits. Dashboard aggregates per project and workspace.

## Cost gates
Phase 0: document starting cloud bill and max allowable new spend. Phase 1: benchmark existing renderer. Phase 2+: only create new always-on resources when validated against workload; prefer on-demand isolated runner for code or remote browser if feasible. GPU audio host optional, independently approved. Infrastructure Blueprints are reviewed before deploy; no surprise paid service creation.

## Risks
Render persistent disks are per-service and not shared between services; use object storage for interchange. Long-running Chromium can compete with Remotion for memory. Untrusted code isolation must not be weakened to save money. Test queue recovery and cleanup so interrupted jobs don't leak storage.
