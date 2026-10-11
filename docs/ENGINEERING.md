# Engineering playbook and release standards

## Repository baseline
Proposed monorepo: apps/web (Next.js), apps/api (a typed API chosen through ADR), packages/contracts, packages/remotion-templates, workers/build, workers/capture, workers/audio, workers/render, infra/, docs/. Avoid scaffolding placeholder runnable services before architecture and CI are reviewed.

## Development workflow
Feature branch -> linked GitHub issue -> tests and docs -> PR -> CI and review -> merge into main -> environment promotion. Product video content is saved in the Studio database/object store and should not require this workflow. Treat changes to engine, shared component libraries or security policy as real deployments.

## Standards
- Pin Remotion/React/Node versions; commit deterministic lockfiles.
- Strict TypeScript with noUnusedLocals; Python static checks if used; integration and contract tests.
- CI: lint, typecheck, unit, manifest fixtures, dependency audits, container scan, test bundle render, ffprobe, screenshot regression tests.
- Secrets only in secret manager/runtime secret storage; NEVER in Git, manifests, capture video or CI artifacts.
- Database migrations reviewed and reversible when practical; structured logs, correlation IDs, rate limits.
- Pull requests require problem, approach, screenshots or demo, tests, risk, rollback plan and related issue.
- Release must include migration, monitoring, runbook, cost effect and rollback evidence.

## Proposed issue taxonomy
type:docs, type:design, type:feature, type:security, type:infra, type:test; phase:0 through phase:5; priority:P0/P1/P2; status via GitHub board if available. Only use labels that exist; linked issue bodies carry phase and priority without labels otherwise.

## Run locally (target, not yet implemented)
`docker compose up -d postgres redis object-store`; `pnpm install`; `pnpm dev`; API migrations and test fixture import. The exact commands will be replaced with verified setup during Phase 0. Do not interpret them as working today.

## Minimum regression suite
Authenticated project CRUD, immutable versions, asset workspace isolation, invalid manifest errors, source snapshot checksum, paused/cancelled job cleanup, retry idempotency, dual format render, incorrect TTS duration detection, browser session teardown, SSRF blocked, signed URL expiration, restore from backup.

## Operations
Health/readiness probes, job heartbeats, bounded logs, worker restart recovery, credential rotation, backup/restore drills, storage garbage collection, cost metrics and on-call runbook before production deployment.
