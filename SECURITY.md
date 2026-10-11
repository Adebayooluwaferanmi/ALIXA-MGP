# Security and responsible media capture

**Status:** pre-production design. Report vulnerabilities privately to the repository maintainers using an agreed nonpublic contact channel; do not open public issues with exploitable secrets or patient data.

Threat areas: source-code execution, npm supply chain, Chromium session isolation, SSRF/DNS rebinding, browser cookies/passwords, credential leakage, cross-workspace object access, signed URLs, model voice consent and unsafe public publishing.

Required before public launch: threat model, isolated build-worker escape tests, storage authorization tests, CSRF/session protections, rate limiting, audit logs, network egress restrictions, dependency pinning, malware scanning, backup/restore drill and incident response runbook.

Never capture live patient health data for demonstrations. Use synthetic test fixtures. Store access tokens in server-side secret storage and never in project manifests or logs.
