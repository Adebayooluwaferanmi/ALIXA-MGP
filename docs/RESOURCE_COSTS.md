# Linux resource and capacity engineering strategy

## Objective

Provide predictable compute and storage consumption across compilation, browser capture, audio generation and video rendering. Prefer reuse and on-demand execution; never weaken code isolation to save money.

## Deployment baseline

ALIXA-MGP targets Linux on a VM, bare-metal host, or OCI-compatible container host. No external hosting account or current marketing renderer is a runtime prerequisite. Capacity values must come from benchmarks on a declared reference machine, not from a provider's marketing plan.

## Resource profile by workload

| Workload | Typical approach | Controls |
|---|---|---|
| Code editing | Browser-side Monaco | Client resources; debounced diagnostics |
| Build | Ephemeral sandboxed Linux worker | CPU, RSS, PID, disk, timeout; content-hash cache |
| Frame preview | Single-frame render | Cache thumbnails; preemptive priority |
| Scene preview | Low-resolution, limited duration | Quality preset, cancellation, concurrency |
| Final render | Dedicated Remotion worker | Concurrent-job cap, frame batching, retry |
| Browser capture | Local MediaRecorder first; isolated remote Chromium when needed | One-session initial cap, idle timeout, egress |
| Narration | Licensed TTS adapter | Per-clip caching; infer-on-demand |
| Music | Licensed track library first | Optional GPU generation by approved policy |
| Storage | S3-compatible object storage | Deduplication, quota, lifecycle retention |

## Scheduler principles

Admission checks budgets before accepting work; prioritize interactive previews, then approved final renders and batch exports subject to fairness. Job attempts use leases and heartbeats. Workers terminate on cancellation/timeouts. Cache keys include source+lockfile+runtime+engine+input fingerprints. Asset transformations and audio generation are cached separately.

## Linux enforcement

Prefer cgroups v2 for CPU/memory/PID limits, Linux namespaces/seccomp via managed OCI sandbox, read-only roots, non-root processes and no Docker socket. Running arbitrary user code requires independently reviewed isolation; ordinary application Docker containers alone are insufficient for unknown adversarial source. Browser egress must deny private/metadata networks.

## Cost telemetry

Persist CPU-seconds, peak memory, source-build seconds, rendered frames, output bytes, storage residency, egress, remote-browser minutes and audio inference seconds. Show usage per workspace and enforce quotas. Display currency estimates only when actual compute pricing is configured.

## Benchmark and release gate

Establish reference machine specification; measure single-frame, 10-second scene and 90-second dual-format rendering, concurrent build isolation, browser memory, voice latency and recovery. Approve worker sizes/concurrency based on observed 95th-percentile latency, failure rate and cost ceilings. Autoscaling is optional, not assumed.

## Failure containment

Guard against runaway Chromium processes, fork bombs, oversized inputs, long TTS clips, abandoned intermediate files and failed multi-GB outputs. Use retention and orphan cleanup. Shared object storage is preferable to multi-container filesystem sharing.
