# Product Requirements Document (PRD)
## ALIXA-MGP — Linux-native media generation platform
**Version:** 1.1 · **Status:** Architectural baseline / not implemented

## 1. Problem statement

Programmable video systems provide expressive composition engines but do not, by themselves, provide the full media-production lifecycle. A finished video can depend on custom code, website footage, images, licensed music, synthesized speech, timelines, build tooling, rendering infrastructure, asset permissions, review and delivery. Without a unified control plane, these resources are coordinated across disconnected applications.

The resulting engineering problems are:
- **Authoring-execution coupling:** creative source changes can trigger full application build and deployment workflows rather than isolated creative-project builds.
- **Media ingestion fragmentation:** screenshots, browser recordings, music and narration are stored separately from the composition they serve.
- **Nonreproducible builds:** mutable input paths, unspecified dependencies and drift in source or generated media undermine reproducibility.
- **High iteration cost:** full-duration rendering is used when frame/scene preview would suffice; duplicate bundle and audio work wastes compute.
- **Execution/security exposure:** programmable media executes JavaScript, accesses URLs and processes untrusted inputs; uncontrolled workers present meaningful security risks.
- **Weak production governance:** no unified job history, asset rights, approval, error diagnosis, render provenance or workspace budgets.

## 2. Product vision

Provide a **Linux-native, self-hostable, browser-accessible media engineering platform** where code, media assets and production intent are first-class versioned resources. Deliver a continuous workflow from authoring and capture to voice, preview, rendering, quality assurance and export. The rendering engine remains replaceable at its boundary; Remotion is the initial video composition engine.

## 3. Scope boundaries

The application is not a hosting-account dashboard, a social-network-specific video generator, or a replacement for professional non-linear editing systems. It is an environment for programmable media production. It may integrate with GitHub and Google Drive, but neither is mandatory for running a project. An ALMS promotional film is a useful reference production, not the platform's identity.

## 4. Users and core journeys

**Motion developer:** create/import TSX source, resolve build errors, inspect a scene, publish an approved bundle.
**Producer:** create project, adjust manifest/timeline, choose assets, preview, request final output.
**Capture operator:** authenticate to an authorized web app, record live footage, redact and attach asset.
**Audio editor:** choose approved narrator, regenerate a scene segment, position music and approve mix.
**Reviewer:** annotate previews, reject/approve output, verify rights and export status.
**Platform operator:** provision Linux worker capacity, set quotas, monitor jobs and security policies.

## 5. Functional requirements

| ID | Priority | Requirement | Verification |
|---|---|---|---|
| FR-01 | Must | Authenticated workspaces with RBAC | Negative access tests across projects |
| FR-02 | Must | Persistent projects and immutable production versions | Revision snapshot and rollback test |
| FR-03 | Must | Remotion template and TSX project ingestion | Compile pinned source without platform deploy |
| FR-04 | Must | Manifest validation, scene coverage and output presets | Invalid manifests rejected before queue |
| FR-05 | Must | Managed uploads and pinned connector imports | Asset SHA, provenance and permissions verified |
| FR-06 | Must | Frame/short-scene previews | Preview without full-film render |
| FR-07 | Must | Durable, cancellable, retryable render jobs | Restart/retry/idempotency tests |
| FR-08 | Must | Signed output playback, QA and download | Completed MP4 with matching metadata |
| FR-09 | Should | Local tab/window capture via browser permission | Captured demo attached to project |
| FR-10 | Should | Interactive isolated Chromium capture | Session teardown and SSRF security tests |
| FR-11 | Should | Scene-level narration generation and approval | Regenerate one line without replacing others |
| FR-12 | Should | Licensed background music and FFmpeg mastering | Probe and listen to mixed output |
| FR-13 | Must for custom-code launch | Sandboxed build/render of custom source | Escape, egress and quota tests |
| FR-14 | Should | Cost telemetry, quotas and cache policy | Budget enforcement and measured utilization |
| FR-15 | Later | AI-guided scene drafting and advanced collaborative editing | Controlled feature acceptance |

A limited trusted-template path may be released before general custom-code admission. General user-authored code **must not** run until isolation controls have passed review.

## 6. Nonfunctional requirements

Linux/OCI portability, deterministic source snapshot IDs, locked dependencies, authenticated storage, TLS, no shared privileged execution, measurable queue latency, bounded RAM/CPU/runtime, incident logging, accessible controls (WCAG 2.2 AA target), backup/restore and documented capacity planning. CPU-first deployment must remain usable without a GPU; GPU audio is optional and isolated.

## 7. Acceptance scenario

A team creates a new media project from a source snapshot, captures an authorized software demonstration, places a recording into a scene, generates or imports approved narration, previews the relevant segment, and renders landscape/portrait outputs. The outputs are downloadable from the results library with source and license provenance. **Changing this creative project does not require redeployment of the platform.**

## 8. Delivery and outstanding decisions

The [roadmap](ROADMAP.md) defines gated releases. Before implementation, resolve sandbox technology, Linux host constraints, object-store choice, authentication strategy, narrator rights, expected usage volume and permitted operating budget. Targets must be benchmarked and approved rather than assumed.
