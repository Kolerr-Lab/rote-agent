# Changelog

All notable changes to this project are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and the project uses
[Semantic Versioning](https://semver.org/) (APIs may change before 1.0).

## [0.1.0] - 2026-11-05

Initial public release.

### Added
- Discovery agent (observe, decide, act) with an OpenAI provider and an offline `MockLlmProvider`.
- Typed capability artifact: Zod schema with exported JSON Schema (`schema/capability.schema.json`).
- Deterministic replay engine with ranked locator resolution, invariant checks and checkpoints.
- Result contract `success | business_outcome | hard_failure | escalation_required` and failure taxonomy.
- Bounded recovery rules (`dismiss`, `retry`, `reauth`).
- Human control lease and handoff on the same live session, plus a minimal operator panel.
- Safety: route and action allowlist, risk policy, redaction of sensitive values.
- Tenant profiles for replaying one artifact across rebranded UIs.
- Agent-callable capability catalog and CLI (`discover`, `replay`, `catalog`, `operator`, `mock-bank`).
- Mock legacy bank with injectable failures and two tenants.
- Documentation (`docs/`), CI workflow, and community files.
