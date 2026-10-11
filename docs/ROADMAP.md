# Delivery roadmap, milestones and team expectations

**Schedule basis:** relative weeks after team kickoff, not hard calendar commitments. Reforecast after discovery and staffing. Target: 18-week implementation, with gated pilot releases.

| Phase | Weeks | Deliverables | Exit gate / demo |
|---|---|---|---|
| 0 — Foundation | 1–2 | PRD, architecture/ADR, threat model, OpenAPI/manifest contract, repository standards, CI skeleton and local dev plan | New team member runs contract checks; risks and budget owner identified |
| 1A — Control plane | 3–4 | Auth/RBAC, projects, versions, PostgreSQL migrations, asset metadata, storage, signed URLs | Create project/version and securely upload/retrieve media |
| 1B — Rendering UX | 5–6 | Template/manifest runner, durable queue, progress, preview, results and downloads | ALMS pilot renders both formats without app redeployment |
| 2 — Capture Studio | 7–9 | Local tab capture, permission UX, screenshot/video, trimming/masking, asset-to-scene; remote isolated browser proof of concept | Authorized ALMS demo produces reusable asset; no live patient data |
| 3 — Audio Studio | 10–12 | TTS adapters, voice profiles, approvals, music library, FFmpeg mix and QA | Fix one mispronounced word, regenerate one clip, rerender with audio |
| 4 — Code-first Pro | 13–16 | Monaco source tree, sandboxed builds, pinned deps, bundle cache, code preview, quota enforcement | Editing custom source does not redeploy platform; isolation tests pass |
| 5 — Harden + pilot | 17–18 | Security testing, recovery drills, load and cost benchmark, documentation and release candidate | Team-led ALMS complete production accepted |

## Deliverable definitions
**MVP after 1B:** authenticated interface + versioned projects + media storage + manifest-driven trusted templates + render queue + previews + downloads. Not all audio or remote browser features are MVP.
**Beta after Phase 3:** browser capture, narrator approval and end-to-end mixed renders.
**V1 after Phase 5:** approved custom code execution, policy controls and operational runbook.

## Critical dependencies
Object storage and auth before import; contract/schema before renderer integration; sandbox design before arbitrary code import; model licensing and representative audio before branded voice release; consent and SSRF controls before remote-browser launch.

## Workstreams / provisional ownership
- Product/Design: workflows, accessibility, acceptance demos.
- Platform/API: data model, RBAC, API, job state machine, observability.
- Media/Frontend: Monaco, Remotion player, timeline, templates.
- Infrastructure/Security: sandbox, secrets, ingress, budgets, backups.
- Capture: browser permissions, Playwright, recording/redaction.
- Audio: TTS providers, timing, licensing, mixing and QA.
One individual may own more than one workstream early; no staffing has been assigned.

## Release gates for each phase
Design reviewed; threat model current; unit/integration tests pass; performance and cost data collected; public claims verified; accessibility check; logs and failure recovery; runbook updated; demo accepted by product owner.

## Risks and mitigations
- **Untrusted code escape** (critical): do not release arbitrary-code mode without isolation penetration tests.
- **Cloud cost creep** (high): queue quotas, pause idle capture, cap render lengths, cache bundles/audio and show estimates from measured metrics.
- **GPU availability** (medium): CPU-friendly voice fallback and provider abstraction; separate external GPU worker if approved.
- **Website capture private data** (high): test accounts, masks, session isolation, publication review.
- **Dependency drift** (medium): lockfiles, pinned runtime, engine compatibility matrix.
- **Provider changes** (medium): GitHub/Drive connector interfaces and immutable snapshots.
- **Team misalignment** (high): definition of done, ADRs, milestone board, demos and release checklists.

## Cadence and tracking
Weekly planning/triage, biweekly demo, explicit owner per issue, prioritized backlog, milestone review at each phase gate. Do not promise dates without scope/team/budget commitment.
