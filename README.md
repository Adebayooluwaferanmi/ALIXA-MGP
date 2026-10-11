# ALIXA-MGP | Media Generation Platform

**A Linux-native, self-hostable environment for programmable media production.**

**Project stage:** Architectural definition · **Status:** Pre-implementation · **Project owner:** ALIXA

## Purpose

Producing high-quality programmatic video requires more than a rendering engine. Creative teams need a unified environment for authoring compositions, acquiring visual media, producing audio, previewing output, controlling compute, and managing reproducible render jobs. In a code-driven production pipeline, these responsibilities are commonly distributed across separate tools and manual handoffs. This fragmentation makes iteration costly, assets difficult to govern, and production outcomes difficult to reproduce.

**ALIXA-MGP** addresses this problem through an integrated media-production control plane and a secure Linux execution layer. It combines editable Remotion projects, browser capture, AI-assisted audio production, asset governance and queued video rendering behind a single web application.

It is a **platform project**, not an ALMS-specific promotional-video tool. ALMS may serve as an acceptance-case project but neither its codebase nor its cloud account determines MGP's design or hosting.

## Intended outcomes

- Author or import React/TypeScript Remotion compositions and render them without redeploying the Media Studio application for each creative revision.
- Capture website screenshots and videos using permissioned local capture or isolated, interactive Chromium sessions; attach captured media directly to the project.
- Organize immutable source revisions, assets, manifests, narration and soundtrack versions into reproducible productions.
- Generate or import narration, manage licensed music, approve takes, align timelines, and create finished audiovisual exports.
- Use scene previews and cached build artifacts to minimize repeated compute.
- Operate on **Linux** using OCI containers, reproducible dependencies, controlled worker processes and provider-neutral infrastructure.

## Users

Motion developers, media producers, designers, software teams producing product walkthroughs, reviewers, and platform administrators. The interface must support both code-first authoring and guided template editing.

## Core subsystems

| Subsystem | Responsibility |
|---|---|
| Project Studio | Projects, versions, storyboards, composition settings, approvals |
| Code Studio | Monaco-based Remotion source editing, build diagnostics, reusable templates |
| Asset Library | Ingestion, provenance, immutable assets, media transformations |
| Browser Capture Studio | Authorized live browser interaction, screenshots and recordings |
| Audio Studio | TTS adapters, approved voice profiles, music assets, mixing and timing |
| Execution Plane | Sandboxed compilation, preview, render jobs, cost/quota controls |
| Results Library | QA, previews, downloads, exports and history |

## Linux-first architecture

```text
Browser UI (Next.js / React / Monaco / Remotion Player)
                         |
            API + policy / PostgreSQL
                         |
               Durable job orchestration
                         |
        +----------------+------------------+
        |                |                  |
 Sandbox build      Capture worker      Audio worker
 (Remotion bundle) (Chromium/Playwright) (TTS/FFmpeg)
        |                |                  |
        +----------------+------------------+
                         |
           Immutable object storage
                         |
            Isolated rendering worker
                         |
              QA / output delivery
```

Deployment target: a Linux host, VM or Linux container environment. Cloud vendors are optional infrastructure providers, not product dependencies. Source repository hosting and media resource connectors are likewise replaceable integrations.

## Documentation map

Start here, in order:

1. [Problem definition and product requirements](docs/PRODUCT_REQUIREMENTS.md)
2. [System architecture](docs/ARCHITECTURE.md)
3. [Delivery roadmap and release gates](docs/ROADMAP.md)
4. [Experience and interface specification](docs/UI_UX_SPEC.md)
5. [Project manifest contract](docs/PROJECT_MANIFEST.md)
6. [Browser Capture subsystem](docs/BROWSER_CAPTURE.md)
7. [Audio subsystem](docs/AUDIO_STUDIO.md)
8. [Resource budgets and capacity strategy](docs/RESOURCE_COSTS.md)
9. [Engineering standards](docs/ENGINEERING.md)
10. [Team onboarding](docs/TEAM_ONBOARDING.md)
11. [Delivery tracker](docs/DELIVERY_TRACKER.md)
12. [Architecture decision register](docs/decisions/ADR-0001-platform-boundaries.md)

## Delivery plan

| Release | Target* | Demonstrable capability |
|---|---|---|
| Foundation | Weeks 1–2 | Contracts, threat model, Linux dev stack and CI |
| Core studio | Weeks 3–6 | Projects, assets, trusted compositions, jobs, preview and downloads |
| Capture | Weeks 7–9 | Local tab capture and protected remote Chromium prototype |
| Audio | Weeks 10–12 | Voice approval, music asset management and mixed render |
| Custom-code execution | Weeks 13–16 | Sandboxed source builds, bundle caching and controlled execution |
| Pilot / hardening | Weeks 17–18 | Security and recovery checks, measured budgets and acceptance tests |

*Relative planning targets from team kickoff; not committed dates. Detailed entry/exit gates: [roadmap](docs/ROADMAP.md).

## Success criteria

A producer must be able to create a project, modify a composition or its input data, capture/import assets, preview scenes, generate/approve audio, submit a final render, and retrieve the result **without a platform deployment for a content revision**. Platform code changes continue to use reviewed software release processes.

All execution must be bounded, observable and secure. Any code supplied through the Studio is potentially executable and must be handled as untrusted until explicitly admitted under an isolation policy.

## Development status

This repository currently establishes the product and engineering specification and tracks delivery through GitHub Issues. It **does not yet contain a functioning application**. The [contributor guide](CONTRIBUTING.md) describes team expectations. No hosting provider or new infrastructure service is required by this documentation milestone.

## Licensing

Licensing has not been determined. The repository is public; contributors must not commit credentials, private captured sessions, unlicensed media or patient data.
