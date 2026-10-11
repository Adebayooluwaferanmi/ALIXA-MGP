# Contributing to ALIXA-MGP

Welcome. Start with README, PRODUCT_REQUIREMENTS, ARCHITECTURE, ROADMAP and ADR-0001.

## Team practices
Choose an existing tracked issue (or propose one), confirm owner/scope/dependencies and demonstrate changes against explicit acceptance criteria. Work on descriptive feature branches. Open pull requests against main. Include screenshots for UI changes, test results, operational effect, dependency updates and security review as needed.

## Definition of done
- Acceptance criteria met and linked in PR.
- Typed API/schema and database migrations reviewed.
- Unit/integration tests pass in CI; security checks pass for auth/code execution/capture.
- UI accessible and tested at responsive sizes; UX states handled.
- Docs and ADR updated when architecture changes.
- No credentials, personal patient data, proprietary media, generated voice samples without rights, or hard-coded billing secrets.
- Resource budgets and cleanup considered. Reproducible release/rollback documented.

## Collaboration
Roles: Product Owner (vision, scope, acceptance), Technical Lead (architecture, code review), Contributors (implementation), Security/Infra Reviewer (isolation and deployment), QA/Design (usability and media quality). These are responsibilities, not current assignments.

Use respectful, actionable code reviews. Mark issues blocked with a specific dependency. Do not deploy paid infrastructure without explicit budget approval. Do not enable remote browser access to unauthorized targets.

## Branching
`feat/<area>-<ticket>`, `fix/<area>-<ticket>`, `docs/<topic>`; squash merge unless a PR warrants preserving commits. Prefer small, tested PRs over large unreviewable feature merges.

## Licensing
ALIXA-MGP has no approved open-source license yet. Contributors must confirm rights to any code, model, audio track or video asset before inclusion.
