# ALIXA-MGP — Alixa Media Generation Platform

**Status:** Planning / foundation · **Owner:** ALIXA · **Started:** October 2026

ALIXA-MGP is a browser-based, code-first media production platform. It enables creators and teams to build, preview, narrate, capture, render, review, and deliver videos **without redeploying the platform every time creative content changes**.

> **Product boundary:** ALIXA-MGP is the media production platform. ALMS (Alixa Laboratory Management Software) is the first pilot customer/project, not the platform itself.

## Why it exists
The ALMS promotional film required repeated editing of Remotion source, GitHub commits, Render deploys, authenticated PowerShell job submissions and manual downloads. MGP separates a **stable execution platform** from **versioned creative project source, manifests and assets**. Code changes to the platform still go through CI/CD; ordinary video revisions do not.

## Product promise
1. Create a project in the browser, choose a template or supply custom Remotion React/TypeScript code.
2. Import project resources through direct upload, GitHub (pinned revision) or Google Drive (snapshot at import).
3. Use an integrated Browser Capture Studio to record authorized website demos and screenshots directly into project assets.
4. Edit and preview scenes, narration, music and composition settings.
5. Validate and compile projects in isolated, resource-limited workers; cache reusable bundles.
6. Queue final portrait/landscape renders and download or export results with provenance and QA details.

## Read first
- [Product requirements and scope](docs/PRODUCT_REQUIREMENTS.md)
- [Architecture and security boundaries](docs/ARCHITECTURE.md)
- [Delivery roadmap and team expectations](docs/ROADMAP.md)
- [UI/UX information architecture](docs/UI_UX_SPEC.md)
- [Manifest contract and sample](docs/PROJECT_MANIFEST.md)
- [Browser Capture Studio](docs/BROWSER_CAPTURE.md)
- [Audio pipeline](docs/AUDIO_STUDIO.md)
- [Resource and cost strategy](docs/RESOURCE_COSTS.md)
- [Engineering and quality gates](docs/ENGINEERING.md)
- [Architecture decisions](docs/decisions/ADR-0001-platform-boundaries.md)
- [Contributor handbook](CONTRIBUTING.md)

## Delivery phases
| Phase | Target relative to kickoff | Exit criterion |
|---|---|---|
| 0 — Foundation | Weeks 1–2 | Architecture, schemas, CI, threat model, local stack and first-team onboarding |
| 1 — Core studio | Weeks 3–6 | Create/import/edit project; managed assets; manifest-driven render, jobs and browser downloads, **no redeploy per video** |
| 2 — Browser capture | Weeks 7–9 | Local-tab capture plus authorized remote-browser prototype and asset-to-scene |
| 3 — Audio production | Weeks 10–12 | TTS adapter, voice approval, licensed music, timing, mixing and final A/V render |
| 4 — Advanced code workflows | Weeks 13–16 | Isolated arbitrary-code builds, cache, version comparison, richer previews and cost controls |
| 5 — Harden and pilot | Weeks 17–18 | Security, performance, recovery, documentation and ALMS pilot acceptance |

These are **planning estimates**, not committed dates or staffing promises. Detailed dependencies, risks and acceptance gates are in [ROADMAP](docs/ROADMAP.md).

## Architecture at a glance
```text
Web Studio (Next.js + Monaco + Remotion Player)
            | authenticated API
Studio API / Postgres / Redis queue
  |            |               |
Asset service  Capture service  Build/render orchestration
  |            |               |
S3-compatible storage       Isolated build runner
            |                    |
            +-- Versioned bundle |
                         Remotion renderer (existing Ordabase service as adapter)
                                      |
                              previews / MP4 / download
Audio providers (TTS + music) -> FFmpeg mastering -> render assets
```

**Security:** User-supplied code must never run in the web/API process or in the privileged renderer without sandboxing. Validate network egress, resource limits, dependency allowlist, secret isolation and tenant boundaries. Never place patient data or login secrets in marketing assets.

## Repository layout (planned)
```
apps/web/             # Next.js Studio UI
apps/api/             # API / orchestration
packages/contracts/   # manifest, API schemas, events
packages/templates/   # trusted Remotion compositions
workers/build/        # isolated compile/bundle
workers/render/       # preview and final video
workers/capture/      # controlled Chromium sessions
workers/audio/        # speech/music adapters, mastering
infra/                # compose, Render Blueprints, policies
docs/                 # product and engineering decisions
```
**No production services have been provisioned by this repository initialization.** See [ENGINEERING](docs/ENGINEERING.md) before building or deploying.

## Governance
Use GitHub Issues for work items and Pull Requests for reviewed implementation. Issues should have scope, dependencies, acceptance criteria, tests, risks and definition of done. Avoid merging untested builds. Record significant trade-offs as ADRs. Roles and review expectations are described in [CONTRIBUTING](CONTRIBUTING.md).

## Success metric
An ALMS marketing producer can create a video project, import/capture assets, revise a scene or narration, preview, render both orientations, review and download outputs **without Git commits, manual API calls or Render redeployments**.

## License
No open-source license is granted yet. Repository visibility is public; treat documentation as public and do not commit tokens, customer data, unlicensed media or private recordings. Licensing must be decided explicitly before distributing production code.
